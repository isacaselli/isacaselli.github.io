---
title: "Tutorial 4: Linux Kernel Character Device Drivers"
date: 2025-04-10 14:00:00 -0300
categories: [Linux Kernel, Drivers]
tags: [kernel, modules, drivers, character devices]
description: My experience learning about Linux character device drivers as part of the Free and Open Source Software Development course at USP.
comments: false
---

This tutorial was more about **understanding concepts** than hands-on, focusing on **character device drivers** in the Linux kernel. It introduced key concepts like **major and minor device numbers**, **file operations** (`open`, `read`, `write`, `release`), and how these drivers interact with user space through special files in `/dev`.

## Character devices and driver structure

I learned how character devices manage sequential data transfer (for example, serial ports or keyboards) and how the kernel maps these devices using **major and minor numbers**. The tutorial also explained how drivers implement **file operations** to handle system calls for these devices, and how they connect these operations to the device using **`cdev` structures**.

## Writing and testing a simple driver

After understanding the theory, I worked through an example **character driver** that allocated a buffer in kernel space and implemented basic file operations. The process involved:

- Adding the driver to **kbuild** (`Kconfig` and `Makefile`).
- Enabling it via **`menuconfig`**.
- Rebuilding the kernel and installing the module in the VM.
- Using `modprobe`, `mknod`, and simple C programs to test **read** and **write** operations.

## Issues faced

Although the tutorial was mostly conceptual, the practical steps turned out to be **the most challenging so far**. During the kernel build, I got stuck with **many unexpected config prompts**, and later, my **VM wouldn’t mount properly**. I suspect I misconfigured some directories related to the VM filesystem running commands from previous tutorials.

I had to backtrack, redo parts of the setup from earlier tutorials, and, with help from the course monitors, finally got the VM working again.

## Key takeaways

- Gained a deeper understanding of **character device drivers** and how they map to `/dev` files.
- Experienced real issues with **kernel configuration** and **VM management**, which helped reinforce the importance of having a solid environment setup.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/char-drivers/)
{: .prompt-info }