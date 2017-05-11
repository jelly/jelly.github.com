---
layout: post
title: "Arch weekly 2017-05-13"
date: 2016-12-05 20:00
comments: false
categories: [Arch, weekly]
---

This is the first edition of Arch weekly, a small weekly post about the news in
the Arch Linux community.

## linux-hardened appears in [community]

After the disappearance of linux-grsecurity from the repos due to the Grsecurity
project stopped providing the required patches [Daniel Micay]((https://www.archlinux.org/people/trusted-users/#thestinger) provided an alternative [linux-hardened](https://www.archlinux.org/packages/community/x86_64/linux-hardened/) in [community]. The package is based on the following [Linux fork](https://github.com/thestinger/linux-hardened) which contains more security patches then the Linux mainline kernel and enables more security configuration options by default such as SLAB_FREELIST_RANDOM.
More information can be found on the
[wiki](https://github.com/thestinger/linux-hardened/wiki) of the project.


## Qt 4 now depends on OpenSSL 1.1

Even after the enormous OpenSSL 1.1 rebuild, not every package in the repository
uses OpenSSL 1.1 yet. Qt 4 currently in [testing] uses OpenSSL 1.1 with 27
packages left in the repository which depend on openssl-1.0.

## Arch-boxes project

An effort has been made by
[Shibumi](https://www.archlinux.org/people/trusted-users/#shibumi) to provide
official Arch Linux docker, vagrant (and maybe ec2) images. Currently there is a
virtualbox and qemu/libvirt option. 

## GCC 7.1 testing repo

An external [testing repository](https://pkgbuild.com/~bpiotrowski/gcc7/) has
been create to test the latest GCC release. This will hopefully mean that GCC
7.1 will be in [testing] soon!

## Security updates of the week

A lot.

