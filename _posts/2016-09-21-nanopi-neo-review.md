---
layout: post
title: "FriendlyARM NanoPi NEO review"
date: 2016-08-21 18:00
comments: false
categories: [nanopi, allwinner, friendlyarm]
---

The [NanoPI NEO](http://www.friendlyarm.com/index.php?route=product/product&product_id=132) is a little 8 dollar ARM device with an interesting form factor and specifications.

* 512/256 MB ram (single slot)
* Cortex-A7 Quad-Core
* USB 2.0
* 100 Mbps ethernet
* 40 x 40 mm board size
* SD card slot

![FriendlyARM NANO](http://www.cnx-software.com/wp-content/uploads/2016/07/Smallest_Allwinner_H3_Board.jpg)

Mainline support
----------------

FriendlyARM provides an UbuntuCore image, but of course I want to run Arch Linux ARM on it.

To use Arch Linux ARM on the NanoPI you will have to compile your own kernel and u-boot, I've used the armv7 tarball from [archlinuxarm.org](https://archlinuxarm.org/platforms/armv7/allwinner/cubieboard-2). The mainline kernel does not support the board yet, so a [posted DTS file](https://groups.google.com/forum/#!topic/linux-sunxi/6TIuUps0xpk) is required on top of the mainline kernel. This will provide a kernel without ethernet support, ethernet support can be added by compiling this [Linux tree](https://github.com/montjoie/linux/commits/sun8i-emac-wip-v4) which hopefully lands in 4.9 or later. The DTS file has to be edited to add ethernet support, just append the following underneath the usbphy node.

{% highlight bash %}
&emac {
       phy-handle = <&int_mii_phy>;
       phy-mode = "mii";
       allwinner,leds-active-low;
       status = "okay";
};
{% endhighlight %}

After setting up the correct .dtb and zImage for the kernel, u-boot has to be compiled. U-boot master contains support for the nanopi_neo and has to be compiled as following.
{% highlight bash %}
make -j8 ARCH=arm CROSS_COMPILE=arm-none-eabi-  nanopi_neo_defconfig
make -j8 ARCH=arm CROSS_COMPILE=arm-none-eabi- 
{% endhighlight %}

Power usage
-----------

The board connected with an ethernet cable uses 5V and ~ 0.10 A which means 5 * 0.10 / 1000 = 0.0005 kWh. Running the nanopi NEO a year will costs 0.0005 * 24 * 365 = 4.38. The dutch price per kWh is ~ 0.22 cents, so 4,38 * 0.22 cents = 1 euro!

Heating issues
--------------

The board seems to have some power issues reported [on the sunxi wiki](http://linux-sunxi.org/FriendlyARM_NanoPi_NEO#Voltage_regulators_.2F_heat), so it's recommended to use a heatsink.

Summary
-------

Overal it looks like a fun, low powered board which can be useful for example running small services: mqtt, taskd server, a webcam server or collecting sensor data using the gpio pins.
