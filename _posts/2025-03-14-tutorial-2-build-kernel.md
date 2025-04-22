---
title: "Tutorial 2: Building and booting a custom Linux kernel for ARM"
date: 2025-03-15 14:00:00 -0300
categories: [Linux Kernel, Compilation]
tags: [kernel, arm64, cross-compilation, virtualization]
description: My experience compiling and booting a custom Linux kernel for ARM64 as part of the Free and Open Source Software Development course at USP.
comments: false
---

Continuing the journey into **Linux kernel development**, the second tutorial focused on compiling and booting a **custom Linux kernel for ARM64** inside a virtual machine (VM). Building the kernel from source was a big step forward — and it gave me a clearer understanding of how the pieces of a Linux system fit together.

## Cloning the kernel tree

The first task was to clone the **Industrial I/O (IIO)** kernel tree, which is a specific branch of the Linux kernel focused on sensors and data acquisition subsystems. This introduced me to the concept of **kernel trees** — repositories that contain different areas of kernel development.

I opted to follow the tutorial’s approach and do a **shallow clone** of the tree to save space and time. Even though the tutorial mentioned the value of having full commit history (to better understand the evolution of the code), I chose to stick with the minimal setup for now.

## Kernel configuration

Once the source code was available, I had to **configure the kernel build process**. This was my first real contact with **kernel configuration files** (`.config`), and it was eye-opening to see how modular and customizable the Linux kernel is.

I used tools like:

- **`defconfig`**, which generates a default configuration for the ARM64 architecture.
- **`localmodconfig`**, which trims down the configuration to only include modules that are actually needed in the VM.

Navigating the **`nconfig`** interface allowed me to make small adjustments, like customizing the kernel version name.

## Cross-compilation

Since my **host machine runs AMD64** but the **target VM uses ARM64**, I needed to set up **cross-compilation**. This was a new concept for me: compiling code for a different architecture than the one my machine runs. Setting up the cross-compiler was straightforward following the tutorial, but I realize I still need to better understand concepts like **compiler triplets** (e.g., `aarch64-linux-gnu-`) and **toolchains**. 

## Building the kernel

With the configuration and cross-compilation environment ready, I proceeded to **build the Linux kernel**. This step took some time (even with an optimized configuration), which gave me a sense of the scale of the kernel’s codebase.

Although I didn't face major technical issues during compilation, I noticed that certain **build dependencies** (like `flex`, `bison`, and `ncurses`) were required — another reminder of how interconnected Linux development tools are.

## Installing modules and booting the kernel

After compiling, I had to **install the kernel modules** inside the VM’s filesystem. This process involved **mounting the VM’s root filesystem** from the host, copying the modules over, and unmounting everything safely.

Finally, I configured the VM to **boot using the custom kernel image** I had just built. Seeing the VM boot successfully into a kernel that I compiled from source was a really rewarding moment — it felt like a tangible step into kernel development!

## Challenges faced

I didn't have much trouble **running the commands**, thanks to the detailed tutorial. However, some parts of the **kernel configuration** and **cross-compilation setup** were harder to fully understand:

- The **.config file** contains hundreds of options, and while I learned how to use tools like `nconfig` to navigate it, many settings still felt obscure.
- The **cross-compilation process** worked, but I followed it more as a recipe than as something I fully grasped. Terms like **compiler triplet** or **toolchain** are areas I know I need to study further.

These conceptual gaps didn’t block my progress, but they highlighted areas I want to explore more deeply as I continue in kernel development.

## Final Thoughts

This tutorial helped me:

- Understand the process of **compiling a Linux kernel**.
- Set up and use **cross-compilation** for ARM64.
- Work with **kernel modules** and manage them inside a VM.
- Gain confidence in **booting a custom kernel** safely.

I still have a lot to learn, especially regarding **kernel internals**, **build systems**, and **cross-compilation**, but this was a great hands-on introduction.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/kernel-compilation/)
{: .prompt-info }
