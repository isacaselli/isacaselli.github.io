---
title: "Feedback and updates from our contribution to Linux Kernel"
date: 2025-05-07 14:00:00 -0300
categories: [Linux Kernel, Contributions]
tags: [kernel, iio, imu, i2c, drivers]
comments: false
---

After sending our v1 patch to `drivers/iio/imu/inv_mpu6050/` in the Linux Kernel, we received feedback from the maintainer very quickly — just one day later.

In his response, he mentioned that the way we had refactored the code didn't make much sense, as we were creating a new function that would only be used in that single file. However, he showed us a better approach for implementing the changes we proposed.

## What We Changed

Following the maintainer’s advice, we decided to move the duplicated code into the existing function `inv_mpu_i2c_master_xfer()`, which now handles starting and stopping the I2C master, waiting for completion, disabling SLV0, and checking for NACK errors (this last part was introduced by us).

While preparing the second version of the patch, I got confused about the proper way to submit it. Instead of updating the original commit, I created a new one based on the previous version, which resulted in a diff relative to v1 instead of the original kernel code.

To fix that, we submitted a v3 with the correct changes.

Here is the resulting version of the function in v3:

```c

 /*
  * i2c master auxiliary bus transfer function.
  * Requires the i2c operations to be correctly setup before.
  * Disables SLV0 and checks for NACK status internally.
  * Assumes that only SLV0 is used for transfers.
  */
static int inv_mpu_i2c_master_xfer(const struct inv_mpu6050_state *st)
{
        /* use 50hz frequency for xfer */
	const unsigned int freq = 50;
	const unsigned int period_ms = 1000 / freq;
	uint8_t d;
	unsigned int user_ctrl;
	int ret;
	unsigned int status;

	/* set sample rate */
	d = INV_MPU6050_FIFO_RATE_TO_DIVIDER(freq);
	ret = regmap_write(st->map, st->reg->sample_rate_div, d);
	if (ret)
		return ret;

	/* start i2c master */
	user_ctrl = st->chip_config.user_ctrl | INV_MPU6050_BIT_I2C_MST_EN;
	ret = regmap_write(st->map, st->reg->user_ctrl, user_ctrl);
	if (ret)
		goto error_restore_rate;

	/* wait for xfer: 1 period + half-period margin */
	msleep(period_ms + period_ms / 2);

	/* stop i2c master */
	user_ctrl = st->chip_config.user_ctrl;
	ret = regmap_write(st->map, st->reg->user_ctrl, user_ctrl);
	if (ret)
		goto error_stop_i2c;

	/* restore sample rate */
	d = st->chip_config.divider;
	ret = regmap_write(st->map, st->reg->sample_rate_div, d);
	if (ret)
		goto error_restore_rate;

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

	return 0;

error_stop_i2c:
	regmap_write(st->map, st->reg->user_ctrl, st->chip_config.user_ctrl);
error_restore_rate:
	regmap_write(st->map, st->reg->sample_rate_div, st->chip_config.divider);
error_disable_i2c:
	regmap_write(st->map, INV_MPU6050_REG_I2C_SLV_CTRL(0), 0);
	return ret;
}
```

## Interacting with mantainers

A few days ago, Marcelo Schmidt answered our e-mail with a few tips on code styling, and afterwards another maintainer from iio (not the first one that answered us) said that he waiting for an answer from the first maintainer. Hopefully, that means that we are on the right track to get ou patch accepted :) 

![Visual Studio Code showing kernel patch options](/assets/img/feedback-kernel.png)