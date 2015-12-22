---
layout: post
title: "Arch Linux ARM on the BananaPi"
date: 2015-12-22 18:37
comments: false
categories: [Archlinux, ARM, BananaPi]
---

Since some time the Banana Pi is supported in the Linux kernel and [U-Boot](http://git.denx.de/?p=u-boot.git;a=commit;h=3340eab26d89176dd0bf543e6d2590665c577423).
[Arch Linux ARM](http://archlinuxarm.org/) does not support the board yet, but it is possible to get it working with an upstream U-Boot.

SD card Image
-------------

First we to create an SD card image, since the BananaPi is also ARMv7 we can use the [Cubieboard 2 installation instructions](http://archlinuxarm.org/platforms/armv7/allwinner/cubieboard-2)
Follow the steps until step 6, "Install the U-Boot bootloader". We will have to compile our own u-boot for the board.

U-Boot
------

The next step is creating a u-boot image.
Make sure you have the following packages installed on Arch Linux.

* git
* arm-none-eabi-gcc # ARM cross compiler

Now follow the following steps.

{% highlight bash %}
git clone git://git.denx.de/u-boot.git
cd u-boot
make -j4 ARCH=arm CROSS_COMPILE=arm-none-eabi- Bananapi_defconfig 
make -j4 ARCH=arm CROSS_COMPILE=arm-none-eabi-
{% endhighlight %}

If everything went fine you should have an U-Boot image: **u-boot-sunxi-with-spl.bin**. Now dd the image to your sdcard, where /dev/sdX is your sdcard.

{% highlight bash %}
sudo dd if=u-boot-sunxi-with-spl.bin of=/dev/sdX bs=1024 seek=8
{% endhighlight %}

Flush buffers

{% highlight bash %}
 sync
{% endhighlight %}

Now mount the sdcard again.

{% highlight bash %}
mount /dev/sdX1 mnt
wget http://pkgbuild.com/~jelle/bananapi/boot.scr -O mnt/boot/boot.scr
umount /dev/sdX1
sync
{% endhighlight %}

Booting & Serial
----------------

Instructions on getting serial working are on the [sunxi wiki](http://linux-sunxi.org/LeMaker_Banana_Pi#Locating_the_UART).
