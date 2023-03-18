---
title: "Nvidia Jetson MIPI CSI-2 Connection Use Case"
header:
    overlay_image: https://images.unsplash.com/photo-1625276254563-f0fbbf66a5e7?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80
    caption: "Photo by [Ryutaro Uozumi
](https://unsplash.com/@ryutarouozumi) on [Unsplash](https://unsplash.com)"
tagline: "Some use case for xavier's MIPI CSI-2 connection."
category: gmsl
tags:
- gmsl
- camera
- nvidia-jetson
---

As a software engineer working with Nvidia Jetson, understanding the various CSI-2 modes and hardware wiring options is crucial for successful camera integration. The choice of wiring can significantly impact the available camera types and bandwidth. In this blog post, we will explore some of the commonly used wiring options for MIPI CSI-2 and their implications.

<!--more-->

## ▪️ Xavier MIPI Lanes

Xavier offers multiple MIPI CSI-2 configurations that can be easily adjusted by hardware engineers to meet the system's camera quantity, bandwidth limitation, and power consumption needs. The available x1, x2, or x4 modes can be selected accordingly. With a total of 8 CSI ports on Xavier, each port has two lanes. In x2 mode, one port can be used individually, while in x4 mode, two ports are combined for use.

Available CSI modes:
- x2 (2 lanes = 5Gbps)
- x4 (4 lanes = 10 Gbps)

![Xavier MIPI lanes](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase/Platforms/NvidiaJetson/Attachments/xavier-mipi-csi-configurations.png)

## ✔️ Use Cases

### I want to connect four 4K@60FPS cameras !

The following diagram shows that you can configure 8 CSI ports into 4 groups of x4 mode, each of which can accommodate up to 10Gbps bandwidth. Assuming that we need to connect 4 4K cameras, and the required bandwidth for a 60 FPS 4K camera is approximately 7.4Gbps, this configuration is suitable.

![4 4k cameras](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase/Platforms/NvidiaJetson/Attachments/mipi-csi2-use-case-10g-4-lanes.excalidraw.svg)

### I want to connect 6 4K@30FPS cameras !

The following chart shows that you can configure 8-port CSI into 2 sets of x4 mode and 4 sets of x2 mode, where x4 can accommodate 10Gbps and x2 can accommodate 5Gbps. If we need to connect 4 cameras at 4K@30FPS and 2 cameras at 4K@60FPS, this configuration is suitable.

![6 4k cameras](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase/Platforms/NvidiaJetson/Attachments/mipi-csi2-use-case-5g-2-lanes.excalidraw.svg)

## ❓ Limitation ?

Yes, there's limitation on the maxmium number of connected cameras. Please refer to the MIPI configuration table. You can see **port F** and **port H** are not allowed configured into x2 mode. That means you'll lose 2 ports when all ports are configured in x2 mode.


![port F and port H](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase/Platforms/NvidiaJetson/Attachments/mipi-csi2-use-case-hybrid.excalidraw.svg)


<i class="far fa-sticky-note"></i> **Note:** Xavier MIPI CSI support virtual channel which allows you to connect more than 6 cameras.
  {: .notice--info}
  {: .text-justify}