---
title: "Tutorial 1: Setting up a test environment for Linux Kernel"
date: 2025-02-27 14:00:00 -0300
categories: [Linux Kernel, Test environment]
tags: [qemu, libvirt, virtualization, kernel]
description: This post summarizes my experience setting up a QEMU and Libvirt test environment for Linux Kernel contributions as part of the Free and Open Source Software Development course at USP.
comments: false
---

As part of the initial steps to contribute to Linux kernel development, the first tutorial focused on setting up a safe and isolated test environment using QEMU and Libvirt. But before I could even start this process, I had to prepare my own machine: I recently bought a new laptop with Windows pre-installed and needed to set up a dual boot with Ubuntu Linux.

## Setting up the dual boot

I managed to install Ubuntu alongside Windows with the help of classmates and professors. However, this step ended up taking longer than expected due to a small mistake — we accidentally allocated too little space for the Windows partition, which caused the installer to freeze and fail. Once we realized the issue, we wiped everything and started over from scratch. The second time, it worked just fine.

## Following the tutorial without kworkflow

After that, I was able to follow the tutorial steps smoothly. My partner and I were randomly selected to follow the version of the tutorial without using `kworkflow`, so some commands were longer or more manual compared to those using the tool.

## Issues

One thing I noticed while working through the tutorial was that some of the commands didn't work right away unless run with `sudo`, even though the tutorial included a section on setting up permissions to avoid this. For example, `virsh` commands sometimes required root privileges on my system.

Even though the tutorial walks through configuring proper permissions (with the `libvirt-qemu` group), I still faced a few situations where commands like `virsh` only worked with `sudo`. This made me realize the importance of understanding Linux user groups and permissions more deeply — I probably missed something during the setup, or maybe my distro handles these permissions slightly differently.

Another small issue I faced was with SSH access to the virtual machine. On my Ubuntu system, I had to manually install the `openssh-server` package to enable SSH connections, since it wasn't installed by default. After installing it, I was able to configure SSH access to the VM as the tutorial described.

## Learning about the testing environment

One of the main lessons from the tutorial was understanding why having a safe and isolated testing environment for kernel development is essential. Tools like QEMU and Libvirt allow you to simulate virtual machines where you can install and test custom kernels without risking your main development machine. This was my first time setting up something like this, and I could clearly see the benefits.

## Final Thoughts

I appreciated that the tutorial encouraged us to take the time to read and understand each command. This mindset definitely helped when troubleshooting small issues or adapting steps to my specific setup. In the end, I successfully launched a virtual machine running Debian ARM64, with a custom kernel and `initrd`, using both QEMU and Libvirt.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/qemu-libvirt-setup/)
{: .prompt-info }