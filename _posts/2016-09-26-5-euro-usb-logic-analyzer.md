---
layout: post
title: "5 euro USB logic analyzer review"
date: 2016-08-26 23:00
comments: false
categories: [logic analyzer, pulseview, arch, linux]
---

I've found this cheap 5 euro USB logic analyzer via [cnx.com](http://www.cnx-software.com/2015/09/27/using-usb123-usbee-ax-pro-5-usb-logic-analyzer-with-pulseview-in-linux/) and bought it from [aliexpress](http://www.aliexpress.com/item/1set-New-Arrival-USB-Logic-Analyze-24M-8CH-MCU-ARM-FPGA-DSP-debug-tool/1971933314.html).
It turned out to be quite easy to get the analyzer working on Arch Linux, the following packages need to be installed:

{% highlight bash %}
pacman -S pulseview
{% endhighlight %}

The fiirmware for the device is only available in the [AUR](https://aur.archlinux.org/packages/sigrok-firmware-fx2lafw/).
{% highlight bash %}
cower -dd sigrok-firmware-fx2lafw
cd sigrok-firmware-fx2lafw && makepkg -si
{% endhighlight %}

To be able to use the logic analyzer in [pulseview](https://sigrok.org/wiki/PulseView) without running it as root, requires an udev rule to be setup. Since libsigrok does not provide this udev rule, which might be considered as a packaging bug of Arch Linux.

{% highlight bash %}
wget http://pkgbuild.com/~jelle/60-libsigrok.rules
# As root / sudo
cp 60-libsigrok.rules /usr/lib/udev/rules.d/
udevadm control --reload
{% endhighlight %}

To test the logic analyzer I soldered a simple board which 'taps' the serial communication from the nanopi neo to my laptop. After connecting the ground and RX, TX from the board to the logic analyzer I started pulseview. Then select the saleae logic (or a different name) as device and press run while making some noise in the program connected to the tty device (for example screen).
![Pulseview 1](https://pics.vdwaa.nl/blog/pulseview1.png)

In pulseview some peaks should show up, it might help to increase the sample size (for example to 1M).
Configuring pulseview to decode UART data is as easy as selecting the UART option from the GUI.

![Pulseview 2](https://pics.vdwaa.nl/blog/pulseview2.png)

Select the connected RX/TX corresponding to how you hooked up them up to the logic analyzer, the baud rate, data bits etc. might be different in your setup.

![Pulseview 3](https://pics.vdwaa.nl/blog/pulseview3.png)

And voila, pulseview decodes the UART data into a readable ascii representation.

![Pulseview 4](https://pics.vdwaa.nl/blog/pulseview4.png)

Summary
-------

This is the first logic analyzer I've ever used and so far it has exceeded my expectations. It was easy to get pulseview working, all the software which is required is fully open source. The analyzer should be able to analyze I2C, SPI and UART, limited up to 24 MHz. So far worth the 5 euro :-)
