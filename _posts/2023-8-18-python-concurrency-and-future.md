---
title: "Python Concurrency and Future"
tagline: "A simple concurrency and future manipulation in python."
category: programming-language
tags:
- python
---

This note describes how to use concurrency and future in python3. (Applies to Python3.7 and above)

<!--more-->

## 📝 Tutorial

### Running Tasks

The `concurrent.futures` module provides a high-level interface for asynchronously executing callables. It supports both thread-based and process-based parallelism.

{% capture notice-1 %}
<i class="far fa-sticky-note"></i> **What is the difference between thread-based and process-based ?**

Thread-based :parallel execution within the same process (process)
Process-based : parallel execution across multiple processor
{% endcapture %}

<div class="notice">{{ notice-1 | markdownify }}</div>

Importing the required modules.

```python
from concurrent.futures import ThreadPoolExecutor
import time
```

Submit tasks to the executor.

```python
def my_task(seconds):
    print(f'Sleeping for {seconds} second(s)...')
    time.sleep(seconds)
    return f'Finished sleeping for {seconds} second(s)'

# Submitting tasks to the executor
with ThreadPoolExecutor() as executor:
	task1 = executor.submit(my_task, 2)
	task2 = executor.submit(my_task, 4)
```

{% capture notice-2 %}
<i class="far fa-sticky-note"></i> **Manipulate the _future_ object !**

```python
task1 = executor.submit(my_task, 2)
```
where task1 is the returned _future_  object. You can get the task status by the following methods.
- task1.running() - is task running ?
- task1.cancelled() - is task cancelled ?
- task1.done() - is task done ?
- task1.result() - result returned from the task !
{% endcapture %}

<div class="notice--warning">{{ notice-2 | markdownify }}</div>

Handle results from submitted tasks.

```python
# Retrieving results from submitted tasks
print(task1.result())  # Blocking call - waits for task completion and returns result
print(task2.result())
```

Using _as_completed_ to retrieve results as tasks complete

```python
# Using as_completed to retrieve results as tasks complete
results = [executor.submit(my_task, seconds) for seconds in [2, 4, 1]]
for future in concurrent.futures.as_completed(results):
    print(future.result())
```

Mapping multiple arguments using map()

```python
# Mapping multiple arguments using map()
results = executor.map(my_task, [2, 4, 1])
for result in results:
    print(result)
```

Or we can use _ProcessPoolExecutor_ as the executor.

```python
with concurrent.futures.ProcessPoolExecutor() as executor:
    # Your code here (similar to ThreadPoolExecutor)
```

That's it! You've now learned the basics of Python concurrency using the `concurrent.futures` module.

{: .notice--warning}

{% capture notice-3 %}
<i class="far fa-sticky-note"></i> **Remember to shutdown the executor !** 

Remember to handle exceptions and gracefully shutdown the executor by calling `.shutdown()` or using a context manager (`with` statement) when you're done with it.
{% endcapture %}

<div class="notice--warning">{{ notice-3 | markdownify }}</div>