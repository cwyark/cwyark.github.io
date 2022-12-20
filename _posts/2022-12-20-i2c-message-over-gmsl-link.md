---
title: "I2C Message over GMSL Link"
header:
    overlay_image: https://images.unsplash.com/photo-1625276254563-f0fbbf66a5e7?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80
    caption: "Photo by [Ryutaro Uozumi
](https://unsplash.com/@ryutarouozumi) on [Unsplash](https://unsplash.com)"
tagline: "GMSL solve conflict I2C address problem in a serial line."
category: gmsl
tags:
- gmsl
- camera
- nvidia-jetson
---

GMSL solve conflict I2C address problem in a serial line.

<!--more-->

## 〰️ I2C Message over GMSL Link

The GMSL protocol allows host controller to control remote serializer and sensor through i2c messages which is passthroughed by GMSL link (control stream).

![I2C Message over GMSL Link](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/Hardware/SerDes/Attachments/i2c-message-over-gmsl-link.excalidraw.svg)

- Downlink (camera -> host controller) 
In front of link side (the link between deserializer and serializer), deserializer decapsulated control message and translate into i2c format, then forward to upstream i2c lines. (SDA and SCL).
- Uplink (host controller -> camera) 
On the uplink direction, host controller generate START bit, followed by device address and message content to deserializer. Deserializer helps to forward it to remote side as well.

## ⚪ Conflict Address on the Same I2C Channel

Each camera modules have identical i2c slave address. For example, in Leopard AR0233 GMSL camera, it's MAX9295 use 0x62 and CMOS sensor use 0x10, it should be the same in other Leopard AR0233 GMSL camera. As a result, when two identical cameras are shared with the same i2c channels, there should have a i2c address conflict issue.

### 💡 Remapping Mechanism

Serializer (e.g. MAX9295, MAX96705) provides a way to change their adddress at runtime, which allows you don't need extra hardware change (e.g strap pin). Simply change by setting serializer's register. (0x01 at MAX9295)

Click here to see [sequence diagram](https://publish.obsidian.md/cwyark-notes/200+KnowledgeBase+%F0%9F%93%93/Hardware/SerDes/I2C+Messages+over+GMSL+Link#%F0%9F%92%A1+Remapping+Mechanism){:target="_blank"} on it works.