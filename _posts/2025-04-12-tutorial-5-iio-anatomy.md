---
title: "Tutorial 5: Dissecting the IIO Simple Dummy Driver"
date: 2025-04-12 14:00:00 -0300
categories: [Linux Kernel, IIO Subsystem]
tags: [kernel, drivers, iio, dummy]
description: Reflecting on the IIO dummy driver tutorial and its role in understanding the internals of sensor drivers in the Linux kernel.
comments: false
---

In this tutorial, we explored the **anatomy of the `iio_simple_dummy` driver**, which serves as a kind of "sandbox" for understanding how sensor drivers are structured within the **Industrial I/O (IIO)** subsystem of the Linux kernel.

Unlike previous tutorials, this one didn’t involve setting up or compiling the kernel. Instead, it focused on **reading and analyzing source code**, making it more conceptual than practical — but still highly valuable. It helped me connect many pieces introduced in earlier tutorials, especially those related to channels, file operations, and driver initialization.

## Key Concepts I Took Away

### IIO Channels and `iio_chan_spec`

At the heart of this driver are the **channels** — abstractions that represent streams of data a sensor might provide. For example, an accelerometer may have three channels (X, Y, Z). Each channel is described using a struct called `iio_chan_spec`, which includes metadata like:

- `.type`: indicates the kind of data (e.g., voltage or acceleration),
- `.indexed` and `.channel`: used to organize multiple channels,
- `.info_mask_separate` and `.info_mask_shared_by_type`: describe what kind of information the channel exposes (like raw values, scale, offset).

This part clarified how **one driver can simulate many different types of sensor behavior** by defining multiple channels with different configurations.

### Read and Write Functions

The tutorial also dives into how data is transferred between the kernel and user space using the functions `iio_dummy_read_raw()` and `iio_dummy_write_raw()`. These functions inspect the **channel type and mask** to decide how to process the request.

It was a good opportunity to understand **how the driver interacts with a "state" struct (`iio_dummy_state`)** to get or set values, and how **locks (mutexes)** are used to protect shared data during access.

### The Probe Function

Finally, the **probe function** ties everything together: allocating memory for the device, initializing data structures, setting up channels, and registering the driver with the kernel. It’s the entry point that transforms definitions and structs into a usable device in the IIO framework.

## Final thoughts

Although I didn’t write or run any code, this tutorial helped clarify how sensor drivers are structured in the kernel and how different components like channels, buffers, and file operations fit together. Reading the iio_dummy code made these ideas more concrete.

> 📚 You can follow the full tutorial [here](https://flusp.ime.usp.br/kernel/iio-dummy/)
{: .prompt-info }