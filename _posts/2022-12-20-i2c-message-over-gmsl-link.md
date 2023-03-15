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

GMSL uses address translation to assign unique virtual I2C addresses to devices on a serial line, resolving conflicts that can arise from multiple devices with the same physical I2C address

<!--more-->

## ▪️ I2C Messages over GMSL Link

The GMSL protocol facilitates I2C communication between a host controller and remote serializer and sensor devices by passing messages through the dedicated control stream within the GMSL link. This allows the host controller to control and manage the remote devices without the need for additional interfaces or wiring, simplifying the design of GMSL systems.

![I2C Messages over GMSL Link](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase/Hardware/SerDes/Attachments/i2c-message-over-gmsl-link.excalidraw.svg)

- Downlink (camera -> host controller) 
In front of link side (the link between deserializer and serializer), deserializer decapsulated control message and translate into i2c format, then forward to upstream i2c lines. (SDA and SCL).
- Uplink (host controller -> camera) 
On the uplink direction, host controller generate START bit, followed by device address and message content to deserializer. Deserializer helps to forward it to remote side as well.

## ⚪ Conflict Address on the Same I2C Channel

Each camera module, such as the Leopard AR0233 GMSL camera, may have identical I2C slave addresses for its MAX9295 serializer and CMOS sensor. This can cause address conflict issues when multiple identical cameras are connected to the same I2C channel. To avoid such conflicts, differentiating methods such as modifying the serializer's address or using an I2C multiplexer can be employed.

### 💡 Remapping Mechanism

Many serializers, such as the MAX9295 and MAX96705, offer the ability to remap their addresses at runtime, eliminating the need for additional hardware changes like strap pins. This can be accomplished by simply changing the serializer's register, providing a more flexible and efficient solution for managing device addresses in a hardware design.

Click here to see [sequence diagram](https://notes.cwyark.me/200+KnowledgeBase/Hardware/SerDes/I2C+Messages+over+GMSL+Link#%F0%9F%92%A1+Remapping+Mechanism){:target="_blank"} on it works.