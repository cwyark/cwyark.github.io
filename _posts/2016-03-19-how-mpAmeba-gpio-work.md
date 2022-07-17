---
title: "How GPIO and I/O interrupt works in MicroPython@Ameba ?"
description: ""
category: tutorial
tags: [python, rtl8195a, micropython, ameba]
---

It's quite simple to handle a GPIO pin in micropython.

<!--more-->
```python
>>> from hardware import Pin
>>> a = Pin("PA_5", dir=Pin.OUT)
>>> a.toggle()
>>> a.value(1)
>>> a.value(0)
```

When you call the method a.toogle(), PA_5 will reverse it's IO status. a.value(x) is to directly to set the IO value. (0 or 1 is available)

Here's my little experiment to see how fast I can to toggle the GPIO ... (The tested pin is PA_5, if you don't know the pin name, check this [plot](http://cwyark.github.io/mpiot/rtl8195a/intro.html#realtek-ameba-board))

```python
>>> from hardware import Pin
>>> a = Pin("PA_5", dir=Pin.OUT)
>>> for i in range(100):
....    a.toggle()
>>>
```

![gpio toggle](/assets/images/micropython-ameba/gpio_toggle.png)

As you can see, each interval between edges are about 26us, which means the interval between every a.toggle() is 26us. I think it's sufficient to simulate a PWM by GPIO, even SPI or I2C protocols.

Also, I/O control can be accompanied with the time module.

```python
>>> import time
>>> for i in range(1000):
...     time.sleep_ms(100)
...     a.toggle()
```

Or read I/O'value periodically.

```python
>>> a = Pin("PA_5", dir=Pin.IN, pull=Pin.OPEN_DRAIN)
>>> for i in range(1000):
...     value = a.value()
...     time.sleep_ms(100)
```

And there's something you should know: unlike Arduino, the I/O voltage range is `0 ~ 3.3V`. I'm not sure it can tolerate `0 ~ 5V`.

### External Pin Interrupt and ISR ###

External I/O interrupt and ISR (interrupt service routine) is supported in micropython@Ameba

``` python
>>> from hardware import Pin
>>> a = Pin("PA_0", dir=Pin.IN)
>>> def show_string(arg):
...     global counter
...     counter += 1
...     print("counter add one !!!")
>>> 
>>> a.irq_register(show_string, Pin.IRQ_RISING)
>>> data = 1    # now give pin PA_0 a rising edge I/O signal
counter add one !!!
>>> print(data)
2
>>> a.irq_unregister()
```

However, there's some limitation when wirting the ISR. Please check this [url](http://docs.micropython.org/en/v1.8/unix/reference/isr_rules.html#critical-sections), the micropython's author has an complete explanation and examples for writing an ISR.

For more informations, you could check this [url](http://cwyark.github.io/mpiot/rtl8195a/modules/hardware.html).