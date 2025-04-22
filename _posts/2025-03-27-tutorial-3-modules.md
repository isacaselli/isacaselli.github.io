---
title: "Tutorial 3: Exploring Linux Kernel Modules and Build Configuration"
date: 2025-03-27 14:00:00 -0300
categories: [Linux Kernel, Modules]
tags: [kernel, modules, kbuild, configuration]
description: My experience creating and configuring Linux kernel modules as part of the Free and Open Source Software Development course at USP.
comments: false
---

In this tutorial, I created a **simple Linux kernel module**, integrated it into the **build system (kbuild)**, and tested it in a virtual machine.

## Kernel modules and build configuration

Kernel modules extend the kernel at runtime without rebooting. I wrote a basic module that logs messages when loaded and unloaded, then added it to the **Kconfig** and **Makefile** to enable it through **`menuconfig`**.

## Building and testing

After enabling the module, I rebuilt the kernel, installed the modules into the VM, and used **`insmod`**, **`rmmod`**, and **`modprobe`** to load and unload it. Checking **`dmesg`** confirmed the module worked as expected.

## Challenges

The tutorial introduced more **advanced terminal tasks** like mounting the VM filesystem and managing modules. I think some steps were not very clear for beginners (like configuration with menuconfig), so I had to read the tutorial more carefully, but I completed everything.

## Key takeaways

- Writing and testing **kernel modules**.
- Configuring builds with **kbuild** and **menuconfig**.
- Loading modules dynamically in a VM.

I also learned how modules can **interact** by exporting and importing functions.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/kernel-modules/)
{: .prompt-info }