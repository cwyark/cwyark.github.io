---
title: "Writing A Monitoring Thread Without Destroying main() Process"
header:
    overlay_image: https://images.unsplash.com/photo-1518770660439-4636190af475?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1740&q=80
    caption: "Photo by [Alexandre Debiève](https://unsplash.com/@alexkixa) on [Unsplash](https://unsplash.com)"
tagline: "writing a monitoring thread without destroying main process."
category: embedded-system
tags:
- embedded-system
- linux-development
---

This post is to highlight the ability to create a monitoring thread within a program's process without disrupting its normal execution in C/C++ program. This is a valuable capability in software development because it allows developers to monitor and analyze the behavior and performance of their applications without interrupting or affecting their functionality.

<!--more-->

Here'a my strategies to do this:

- Use `__attribute__((constructor))`
- Create a static library which will be linked to arbitrary main function
- Fork a child process and create a monitoring thread in your constructor

## ▪️ __attribute__((constrictor)), That's it

Let's say you have a `constructor`  function which allocates some resources and initialize data that you don't want to do them in your main program. We can leverage `gcc` or `clang`'s `__attribute__((constructor))` to achieve this.

Please refer to this [post](https://notes.cwyark.me/200+KnowledgeBase/Programming/C_C%2B%2B/Run+Some+Code+Before+main()+Function+in+C+or+C%2B%2B), this makes your code running before `main()`.

```cpp
#include <stdio.h>
void constructor() __attribute__((constructor)) {
	printf("This function is executed before main()");
}
```

Then compile the `constructor` function into a `static library`.

```bash
$> gcc -c my_constructor.c -o my_constructor.o
$> ar rcs libmy_constructor.a my_constructor.o
```

## ▪ Link You Code to Arbitrary Program

Once you complete your `static library`, you can use `linker` to link it to your main program.

_main.c_
```cpp
#include <atomic>
#include <iostream>
std::atomic<bool> running_ = true;
int counter = 0;
int main() {
	while (running_) {
		std::cout << "hello world. counter = %d" << counter++ << std::endl;
	}
}
```

```bash
$> gcc main.c -L. -lmy_constructor -o main
```

## ▪️ Child Process Mangement

The final step is to fork this process to duplicate a calling process and create a monitoring thread to monitor the child process. This approach allows us to perform monitoring tasks within a separate thread of executions.

_Modify your constructor_

```cpp
class Monitor{
 public:
	 Monitor(pid_t pid), pid_(pid) {
    this->running_ = true;
    this->thread_ = std::thread(&Monitor::worker, this);
  };
  ~Monitor() {
    {
      std::unique_lock<std::mutex> lock(this->mutex_);
      this->running_ = false;
    }
    if (this->thread_.joinable()) {
      this->thread_.join();
    }
  }
  void worker() {
    while (true) {
      if (!this->running_) {
        return;
      }
      if (this->isChecked()) {
				// do something when some criteria met.
      } else {
				// do something when some criteria not met.
      }
      std::this_thread::sleep_for(3s);
    }
  }

	bool isChecked() {
		// return true when some criteria meets and other wise.
	}

 private:
  std::thread thread_;
  std::mutex mutex_;
  std::atomic<bool> running_;
  pid_t pid_;
};

static __attribute__((constructor)) void init(void) {
  child_pid_ = fork();
  if (child_pid_ == -1) {
		std::cout << "failed on creating child process." << std::endl;
    exit(1);
  } else if (child_pid_ > 0) {
    // this is a parent process.
    auto mn = std::make_unique<Monitor>(child_pid_);

    int status;
    waitpid(child_pid_, &status, 0); // blocking wait here until child process is finished.
    if (WIFEXITED(status)) {
      logger_.info("child process exit with %d", WEXITSTATUS(status));
    } else if (WIFSIGNALED(status)) {
      logger_.info("child process exit with %d", WTERMSIG(status));
    }
    mn.reset();
    exit(0);
  }

  return;
}
```