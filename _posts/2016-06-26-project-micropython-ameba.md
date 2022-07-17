---
title: "[Project] Micropython@RTL8195A ports"
tagline: "A novel way to develope application on RTL8195A"
description: "A novel way to develop application on RTL8195A"
category: projects
tags: [ameba, micropython, python, rtl8195a]
---

MicroPython@RTL8195A is an alternative platform of [PyBoard](https://micropython.org/) and [Esp8266](https://www.kickstarter.com/projects/214379695/micropython-on-the-esp8266-beautifully-easy-iot). Developers can write their own python script instead of native C language or Arduino C. MicroPython is python users or newbies friendly platform to dive into the microcontroller, especially for wireless products.

![MicroPython@RTL8195A](/assets/images/micropython-ameba/rtl8195a_first_look.png){: .full}

<!--more-->

Ameba board is based on Realtek RTL8195A SoC. RTL8195A features in WiFi, NFC, and other peripherals like GPIO, SPI, I2C, ADC, DAC, PWM, SDIO and H/W crypto engine.
Ameba board supports Arduino SDK, however, when I'm developing an application, I often compile, program, console printf, compile, program and console printf .... that is really waste of time. So I start to port micropython to Ameba board. By the help of micropython@RTL9105A, developers can facilitate developing applications instead of trouble shooting with compiled language.

Check the full document here: [mpiot/RTL8195A](http://cwyark.github.io/mpiot/rtl8195a/install.html)

And the forked repository is in here , please feel free to leave you comments or pull request: [github](https://github.com/cwyark/micropython)

Related posts

<ul>
    {% for post in site.posts %}
        {% if post.project contains "micropython_ameba" %}
            <li>
                <a href="{{ post.url }}">{{ post.title }}</a>
            </li>
        {% endif %}
    {% endfor %}
</ul>