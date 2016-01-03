---
layout: post
title: "BananaPi with Arch Linux ARM and a mainline kernel"
date: 2015-12-30 20:37
comments: false
categories: [Archlinux, ARM, BananaPi, Linux]
---

I posted a guide on getting [Arch Linux ARM on the BananaPi](http://vdwaa.nl/archlinux/arm/bananapi/banana-pi-archlinux-arm/) last week. Now I was eager to get a mainline kernel working on the BananaPi for some ARM hacking and testing of new patches. In this post I'll describe the steps required to get a mainline kernel booted next to the "normal" kernel.

Compiling mainline kernel
-------------------------

I am using Hans de Goede's branch here, since he has some patches for USB OTG support for the BananP in there. But the normal mainline
kernel should work with similiar steps

{% highlight bash %}
git clone https://github.com/jwrdegoede/linux-sunxi.git
cd linux-sunxi
git checkout -B sunxi-wip origin/sunxi-wip

wget https://fedorapeople.org/~jwrdegoede/kernel-driver-programming/kernel-config
mv kernel-config .config
# CROSS_COMPILE may differ per distro. arm-linux-gnu- probably works for the other distro's.
make -j4 ARCH=arm CROSS_COMPILE=arm-none-eabi- oldconfig 
make -j4 ARCH=arm CROSS_COMPILE=arm-none-eabi- dtbs
make -j4 ARCH=arm CROSS_COMPILE=arm-none-eabi- zImage
{% endhighlight %}

Now that we have a kernel compiled, mount the sdcard.

{% highlight bash %}
mount /dev/sdX1 mnt

cp /path/to/linux-sunxi/arch/arm/boot/zImage mnt/boot/zImagemainline

mkdir mnt/boot/dtbsmainline
cp /path/to/linux-sunxi/arch/arm/boot/dts/*.dtb mnt/boot/dtbsmainline/
umount /dev/sdX1
sync
{% endhighlight %}

Now that we have copied the mainline kernel and dts files to the sdcard, it still won't boot it.
Since the boot.scr is specified to look dts/\* and zImage. So we will need to copy
a new scr file. (*note that you cannot edit the scr file on the sdcard*)
You can either generate a new scr file or create on yourself!

{% highlight bash %}
mount /dev/sdX1 mnt
wget http://pkgbuild.com/~jelle/bananapi/boot-mainline.scr -O mnt/boot/boot.scr
umount /dev/sdX1
sync
{% endhighlight %}

Or generate a new scr file, which sounds more fun.

{% highlight bash %}
pacman -S uboot-tools

wget http://pkgbuild.com/~jelle/bananapi/boot.cmd
mkimage -A arm -O linux -T script -C none -a 0 -e 0 -n "BananPI boot script" -d boot.cmd boot.scr

mount /dev/sdX1 mnt
cp boot.scr mnt/boot/boot.scr
umount /dev/sdX1
sync
{% endhighlight %}

I haven't figured out what the boot.scr exactly does or if there is a way to specify which kernel to boot with one boot.scr file.
So expect some more posts while I explore more about U-boot and ARM :-)
