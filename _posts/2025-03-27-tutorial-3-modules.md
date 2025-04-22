---
title: "Tutorial 3: Exploring Linux Kernel Modules and Build Configuration"
date: 2025-03-27 14:00:00 -0300
categories: [Linux Kernel, Modules]
tags: [kernel, modules, kbuild, configuration]
description: My experience creating and configuring Linux kernel modules as part of the Free and Open Source Software Development course at USP.
comments: false
---

After building and booting a custom Linux kernel in the previous tutorial, the next step in the journey was to **explore Linux kernel modules**. This tutorial introduced the process of creating a simple module, configuring its build setup, and dynamically loading and unloading it in the kernel.

## Kernel modules: a first encounter

Kernel modules are pieces of code that can be loaded into or unloaded from the kernel at runtime, extending its functionality without needing a reboot. This concept was new to me in theorya nd ut this was my first real experience writing and working with them.

I followed the tutorial to create a **simple kernel module** that printed messages when loaded and unloaded. Writing the C code for the module was straightforward — it consisted of two basic functions (`init` and `exit`) that used `pr_info` to log messages to the kernel log.

## Configuring the kernel build

One of the key steps was to integrate this module into the **kernel build system (kbuild)**. This required:

- Adding an entry in the **Kconfig** file to make the module configurable via `menuconfig`.
- Editing the **Makefile** to ensure the module would be compiled based on the configuration.

These steps helped me understand how the kernel organizes its build system, and it was good to know the **best practices** like keeping entries alphabetically ordered in these files.

## Using menuconfig

With the module integrated into the build system, I used **`menuconfig`** to enable it as a loadable module. This process felt more intuitive than I expected — searching for the module symbol, enabling it, and saving the configuration was simple. Still, navigating through the interface felt overwhelming at times since it was my first time doing this.

## Compiling and testing the module

After configuring, I **rebuilt the kernel and the modules**. The tutorial guided me through cleaning previous build artifacts, compiling everything again, and installing the modules into the VM’s filesystem.

One key part of the process was **loading the module inside the running VM**. Using commands like `insmod`, `rmmod`, and `modprobe`, I was able to insert and remove the module dynamically, and check the kernel logs (`dmesg`) to verify that the expected messages appeared. This confirmed that the module was working as intended.

## Challenges faced

While the previous tutorials were more about understanding concepts, this one pushed me further into **hands-on terminal work**. Some of the **commands felt more advanced** for me as a beginner, especially those related to **module handling** and **mounting the VM filesystem**. I had to revisit some concepts like how **`guestmount`** works and how to properly **install modules** in the VM environment.

At certain points, the tutorial assumed a level of familiarity with these tools that I didn’t fully have yet, which led me to do some extra reading to understand what was happening. Even so, I was able to follow along and complete all the steps.

## Final Thoughts

This tutorial helped me:

- Understand the **Linux kernel module system**.
- Configure and compile kernel modules with **kbuild**.
- Dynamically **load and unload modules** in a running kernel.
- Use **menuconfig** to manage kernel features.

It also showed me how **modules can interact**, by exporting functions from one module and calling them in another. This gave me a glimpse into how different components in the kernel ecosystem can be modular yet interconnected.

Although some parts of the process (especially around module installation and testing) were more challenging due to my limited experience, this hands-on approach has been invaluable for building confidence in **kernel development workflows**.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/kernel-modules/)
{: .prompt-info }