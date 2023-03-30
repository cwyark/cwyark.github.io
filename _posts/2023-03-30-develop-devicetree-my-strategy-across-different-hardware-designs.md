---
title: "Developing Devicetree - My Strategy Across Different Hardware Designs"
header:
    overlay_image: https://images.unsplash.com/photo-1518770660439-4636190af475?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1740&q=80
    caption: "Photo by [Alexandre Debiève](https://unsplash.com/@alexkixa) on [Unsplash](https://unsplash.com)"
tagline: "My strategy on developing device tree."
category: embedded-system
tags:
- embedded-system
- linux-development
---

This post is to share my strategy when developing devicetree across different hardware designs.

<!--more-->

## ▪️ Staying Organized When Working On Multiple Projects

When a new processor is released, hardware companies design different hardware circuits based on this processor to meet customer needs. Sometimes, in consideration of product planning, the design of multiple project parts is often shared to achieve the minimum amount of modification in the future. As a project manager, it is important to ensure efficient use of resources and minimize rework. `As an firmware engineer, it is important to find ways to quickly respond to new task requirements among multiple projects.`

![Projects with same feature](https://publish-01.obsidian.md/access/ae397ad8ea8e6bd32fc8c4933cc15acb/300%20Self-Improvement/Writing/Attachments/share-the-same-hardware.excalidraw.svg)

`devicetree` is an useful tool to manage your BSP projects.

<i class="far fa-sticky-note"></i> **What is devicetree?** A device tree is a data structure used by the Linux kernel to describe the hardware components of a system in a vendor-independent way. It provides a way for the kernel to discover and configure devices without relying on platform-specific code.
  {: .notice--info}
  {: .text-justify}

Here are some advantages of using `devicetree`:

- `Hardware abstraction`: Device Tree provides a hardware abstraction layer, allowing the kernel to be more easily ported across different platforms without the need to modify the kernel source code.
- `Dynamic device configuration`: Device Tree allows for dynamic configuration of devices, which means that the system can be updated without needing to recompile the kernel.
- `Improved system boot time`: Device Tree reduces the kernel boot time by providing a preconfigured list of devices and their configurations, eliminating the need for the kernel to probe for hardware at boot time.
- `Simplified driver development`: Device Tree provides a standardized way to describe the hardware, making it easier to develop drivers that are portable across different platforms.

## ▪️ Recognizing Similarities and Differences

Recognizing similarities and differences among components is crucial for effective project management. In the context of firmware or BSP development, a tool that can be particularly helpful is devicetree overlay. This tool allows for the description of hardware components in a standardized way, which can simplify the development of drivers and make it easier to port the kernel across different platforms. By leveraging the information provided by devicetree, developers can more easily identify commonalities and divergences among components and manage their projects more efficiently.

## ▪️ Using Device Tree Overlay

You many have a base `dts` file (e.g. `project_A.dts`) describing A project.

```
/dts-v1/;
 
/ {
    model = "My Embedded System";
    compatible = "acme,my-system";
 
    memory {
        reg = <0x80000000 0x40000000>;
    };
 
    soc {
        #address-cells = <1>;
        #size-cells = <1>;
        compatible = "simple-bus";
 
        mydevice@10000000 {
            compatible = "acme,my-device";
            reg = <0x10000000 0x100>;
 
            interrupt-parent = <&intc>;
            interrupts = <2 0>;
        };
 
        intc: interrupt-controller {
            #interrupt-cells = <2>;
            compatible = "acme,intc";
            interrupt-controller;
        };
				i2c0 {
					#address-cells = <1>;
	        #size-cells = <0>;
	        compatible = "i2c-gpio";
	        gpios = <&gpio1 0 GPIO_ACTIVE_HIGH>;
	        clock-frequency = <100000>;
		    };
    };
};
```

Now, you have a i2c device `lm75a` is connected in `i2c0` channel in project B. For this, you can create a dtsi specific for this project (e.g. `project_B.dtsi`)

```
/dts-v1/;
/plugin/;

/ {
    fragment@0 {
        target = <&i2c0>;

        __overlay__ {
					lm75a@48 {
	          compatible = "ti,lm75a";
	          reg = <0x48>;
            #temperature-cells = <1>;
          };
        };
    };
};
```

At the end, we can compile your project B's dtbo (`project_B.dtbo`) from the base `project_A.dts` and the overlay file `project_B.dtsi`

```bash
$> dtc -O dtb -o project_B.dtbo project_A.dts project_B.dtsi
```

The method to load dtbo may vary on different platforms, so you should use the appropriate platform-specific method for your specific platform.


## ▪️ Conclusion

This post explains how to utilize devicetree overlay to effectively manage projects. Thanks to its portability feature, engineers can maintain a core dts file and adjust dtbi files based on project requirements, streamlining the development of drivers.