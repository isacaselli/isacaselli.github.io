---
layout: post
title: 'Tutorial 1: Setting up a test environment for Linux Kernel'
date: 2025-02-26 14:00:00 -0300
categories: [Linux Kernel, Test environment]
comments: false
pin: true
description: As part of the Free and Open Source Software Development course at USP, this post summarizes the steps to set up a QEMU and Libvirt test environment for Linux Kernel contributions.
---
As a first step, since I had recently bought a new laptop that came with Windows pre-installed, I had to set up a dual boot system and install Ubuntu Linux alongside it. I did this with help from classmates and professors. However, this step ended up taking longer than expected because of a small mistake — we accidentally allocated too little space for the Windows partition, which caused the installer to freeze and fail. Once we realized the issue, we wiped everything and started over from scratch. The second time, it worked just fine.

After that, I was able to follow the tutorial steps smoothly. My partner and I were randomly selected to not use kworkflow, so we followed the version of the tutorial without it — meaning a few commands were longer or more manual compared to those using the tool. Some of the commands in the tutorial didn’t work right away unless run with sudo, which was a small detail we had to adjust, but once we figured that out, everything worked properly.

The tutorial helped me understand why it’s so important to have a safe and isolated testing environment for kernel development. Tools like QEMU and Libvirt are essential here — they allow you to simulate a virtual machine where you can install custom kernels without risking your actual development machine. It was interesting to see how permissions, groups, and image formats all play a role in getting this setup right.

One of the key parts that helped tie everything together was the activate.sh script. It made it easier to keep all the environment variables, paths, and even custom functions organized while testing things. Writing and using this script also made me think more carefully about what each part of the setup was doing — especially around image creation, resizing, and VM launching.

I appreciated that the tutorial encouraged us not just to copy and paste blindly, but to take a moment and try to understand the commands being used. That mindset definitely helped when I had to troubleshoot small issues or adapt steps to my specific setup.

In the end, I managed to successfully launch a virtual machine running Debian ARM64, with a custom kernel and initrd, using both QEMU and Libvirt. The process was a bit challenging at times, but it really gave me a better understanding of the tools and practices used in real kernel development workflows.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/qemu-libvirt-setup/)
{: .prompt-info }
