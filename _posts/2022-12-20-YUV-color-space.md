---
title: "YUV Color Space"
header:
    overlay_image: https://images.unsplash.com/photo-1525909002-1b05e0c869d8?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1935&q=80
    caption: "Photo by [
David Pisnoy](https://unsplash.com/@davidpisnoy) on [Unsplash](https://unsplash.com)"
tagline: "This post tells you what is YUV color format and it's sub-categories."
category: computer-vision
tags:
- camera
- computer-vision
---

This post tells you what is YUV color space and it's sub-categories.

<!--more-->

## 🌁 YUV Color Space

YUV defines each pixel has:

- one Y (_luminance_) vector
- two _chrominance_ vectors, where..
    - U is _blue projection_
    - V is _red projection_

**luminance**:

![luminance](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/luminance-illustratiion.png){:height="100px" width="400px"}

**chrominance** (red and blue projection):

![chrominance](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/chrominance-illustration.png){:height="100px" width="400px"}

## 🔍 Common YUV formats

### YUV4:4:4

Each pixel contains `Y`, `U` and `V` compoments and each compoments are `1 byte`.

![yuv4:4:4](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/color-format-yuv444.excalidraw.svg)

### YUV4:2:2

Each 2 horizontal pixels contains 2 Y, 1 U and V compoments and both pixels share the same U and V

![yuv4:2:2](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/color-format-yuv422.excalidraw.svg)

### YUV4:2:0

Each 2 `horizontal` and 2 `vertical` pixels (total 4 pixels) contains 4 `Y`, 1 `U ` and 1 `V` compoments and four pixels share the same `U` and `V`. 

![yuv4:2:0](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/color-format-yuv420.excalidraw.svg)

### YUV4:1:1

Each 2 `horizontal` and 2 `vertical` pixels (total 4 pixels) contains 4 `Y`, 1 `U ` and 1 `V` compoments and four pixels share the same `U` and `V`. 

![yuv4:1:1](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/200%20KnowledgeBase%20%F0%9F%93%93/ComputerVision/Attachments/color-format-yuv411.excalidraw.svg)