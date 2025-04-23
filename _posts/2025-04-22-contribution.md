---
title: "Refactoring Auxiliary I2C Transfers in the MPU6050 Driver"
date: 2025-04-20 14:00:00 -0300
categories: [Linux Kernel, Contributions]
tags: [kernel, iio, imu, i2c, drivers]
description: My first patch submission to the Linux kernel, refactoring auxiliary I2C transfers in the MPU6050 IMU driver, as part of the Free and Open Source Software Development course at USP.
comments: false
---

Working with Rodrigo, I’ve submitted my **first contribution to the Linux kernel**! Our patch focuses on improving the **MPU6050 IMU driver**, which handles an accelerometer and gyroscope via the **I2C protocol**. This device lives in the **IIO (Industrial I/O) subsystem**, under `drivers/iio/imu/inv_mpu6050/`.

## What We Changed

Based on the sugestions of last class, we chose the one with two functions `inv_mpu_aux_read` and `inv_mpu_aux_write` sharing the **same sequence of I2C transfer steps**. This sequence involved:

- Starting an **I2C transfer**.
- Disabling the slave device.
- Checking for transfer errors.
- Handling errors and cleanup if something went wrong.

This block of code appeared almost identically in both functions, which we saw as an opportunity to **refactor**.

We introduced a helper function, `inv_mpu_aux_exec_xfer`, to encapsulate this repeated logic. This function:

- Initiates the I2C transfer.
- Disables the slave after the transfer.
- Checks for any transfer errors and returns an appropriate status.

With that in place, the `read` and `write` functions became simpler and **easier to maintain**:

```c
int inv_mpu_aux_read(const struct inv_mpu6050_state *st, uint8_t addr,
                     uint8_t reg, uint8_t *val, size_t size)
{
    /* [...] previous code */
    ret = inv_mpu_aux_exec_xfer(st);
    if (ret)
        return ret;

    /* read data in registers */
    return regmap_bulk_read(st->map, INV_MPU6050_REG_EXT_SENS_DATA,
                            val, size);
}

int inv_mpu_aux_write(const struct inv_mpu6050_state *st, uint8_t addr,
                      uint8_t reg, uint8_t val)
{
    /* [...] previous code */
    ret = inv_mpu_aux_exec_xfer(st);
    if (ret)
        return ret;

    return 0;
}
```

With the function that we created, we got the following:

```c
 /**
  * inv_mpu_aux_exec_xfer() - executes i2c auxiliary transfer and checks status
  * @st: driver internal state.
  *
  *  Returns 0 on success, a negative error code otherwise.
  */
int inv_mpu_aux_exec_xfer(const struct inv_mpu6050_state *st)
 {
	 int ret;
	 unsigned int status;
	 
	 /* do i2c xfer */
	 ret = inv_mpu_i2c_master_xfer(st);
	 if (ret)
		 goto error_disable_i2c;
 
	 /* disable i2c slave */
	 ret = regmap_write(st->map, INV_MPU6050_REG_I2C_SLV_CTRL(0), 0);
	 if (ret)
		 goto error_disable_i2c;
 
	 /* check i2c status */
	 ret = regmap_read(st->map, INV_MPU6050_REG_I2C_MST_STATUS, &status);
	 if (ret)
		 return ret;
	 if (status & INV_MPU6050_BIT_I2C_SLV0_NACK)
		 return -EIO;
 
 error_disable_i2c:
	 regmap_write(st->map, INV_MPU6050_REG_I2C_SLV_CTRL(0), 0);
	 return ret;
 }
 
int inv_mpu_aux_read(const struct inv_mpu6050_state *st, uint8_t addr,
                     uint8_t reg, uint8_t *val, size_t size)
{
    /* [...] previous code */
    ret = inv_mpu_aux_exec_xfer(st);
    if(ret)
        return ret;

    /* read data in registers */
    return regmap_bulk_read(st->map, INV_MPU6050_REG_EXT_SENS_DATA,
                            val, size);
}

int inv_mpu_aux_write(const struct inv_mpu6050_state *st, uint8_t addr,
                      uint8_t reg, uint8_t val)
{
    /* [...] previous code */
    ret = inv_mpu_aux_exec_xfer(st);
    if(ret)
        return ret;

    return 0;
}
```
## Submitting the Patch

Besides working on the code itself, I also experienced how to **submit a patch to the Linux kernel via email**. This is a crucial part of the contribution process, and even though our patch was small, learning this workflow was important.

During this process, I ran into an issue with my **university email address**, which caused some trouble sending emails in the proper format. Thankfully, with help from one of the TAs, I was able to use my **personal email** to send the patch instead.

We also received **feedback from the course TA** after sending the patch for initial review. Some **improvements were suggested** before we submit it officially to the kernel mailing list, and working on these adjustments will be the **next step** in this process.