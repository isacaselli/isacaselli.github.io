---
title: "Starting My Linux Kernel Contributions"
date: 2025-04-18 14:00:00 -0300
categories: [Linux Kernel, Contributions]
tags: [kernel, contributions, iio, imu, drivers]
description: Documenting my first steps contributing patches to the Linux kernel as part of the Free and Open Source Software Development course at USP.
comments: false
---

After several weeks setting up the environment, reading tutorials, and experimenting with kernel builds, **I’m finally at the stage of contributing to the Linux kernel**. With the guidance of the teaching assistants (TAs), who are much more familiar with the contribution process, we’ve been introduced to a few beginner-friendly tasks.

Rather than sticking to the simplest options (like fixing code style issues), my partner Rodrigo and I decided to **explore contributions that would involve working with real kernel logic**

We selected **two possible contributions** to focus on, but we may not complete both — they serve as starting points for us to study the codebase and get involved in the process.

## First option: Claiming direct mode for an IIO device

The first potential patch involves adding proper locking to a light sensor driver within the **IIO (Industrial I/O) subsystem**. The idea is to use the functions `iio_device_claim_direct()` and `iio_device_release_direct()` to ensure exclusive access when the device is in **direct mode**, preventing conflicts between buffered data captures and direct register accesses.

This change would apply to the **ISL29125 light sensor driver** (`drivers/iio/light/isl29125.c`), introducing us to **concurrency control mechanisms** within kernel drivers.

## Second option: Refactoring the MPU6050 IMU driver

The second contribution we considered is a **refactor** in the **MPU6050 IMU (Inertial Measurement Unit) driver**. Specifically, two functions that handle **I2C auxiliary reads and writes** (`inv_mpu_aux_read` and `inv_mpu_aux_write`) contain **duplicated logic** for managing I2C transfers.

The idea here is to **extract the shared code** into a separate helper function, improving the driver's **readability and maintainability**. While this task doesn't add new functionality, it supports kernel code quality — an important aspect of contributing to large projects.

## Looking ahead

These are our **first steps into kernel development**, and while we’re still deciding which contribution to move forward with, both options are helping us:

- Understand **locking mechanisms** and **I2C operations** within the IIO subsystem.
- Gain exposure to **real driver code** in the kernel.
- Learn about **kernel contribution workflows**.

I’ll document the process of whichever patch we choose, including any feedback or revisions along the way. Whether the patch gets accepted or not, it’s all part of the learning experience!

> 📚 More updates to come as we move forward — fingers crossed for a successful contribution!