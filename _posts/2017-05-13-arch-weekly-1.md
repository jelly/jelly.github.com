---
layout: post
title: "Arch weekly #1"
date: 2017-05-17 20:00
comments: false
categories: [Arch, weekly, news]
---

This is the first edition of Arch weekly, a small weekly post about the news in
the Arch Linux community. Hopefully this will be a recurring weekly blog post!

## linux-hardened appears in [community]

After the disappearance of linux-grsec from the repos due to the Grsecurity
project not providing the required patches. [Daniel Micay](https://www.archlinux.org/people/trusted-users/#thestinger) provides an alternative [linux-hardened](https://www.archlinux.org/packages/community/x86_64/linux-hardened/) in [community]. The package is based on the following [Linux fork](https://github.com/thestinger/linux-hardened) which contains more security patches than in the Linux mainline kernel and enables more security configuration options by default such as SLAB_FREELIST_RANDOM.
More information can be found on the
[wiki](https://github.com/thestinger/linux-hardened/wiki) of the project.

## Arch-boxes project

An effort has been made by
[Shibumi](https://www.archlinux.org/people/trusted-users/#shibumi) to provide
official Arch Linux docker, vagrant (and maybe ec2) images. Currently there is a
virtualbox and qemu/libvirt option. View the project [here](https://github.com/shibumi/arch-boxes).

## Qt 4 now depends on OpenSSL 1.1

Even after the enormous OpenSSL 1.1 rebuild, not every package in the repository
uses OpenSSL 1.1 yet. Qt 4 currently in [extra] uses OpenSSL 1.1 with 27
packages left in the repository which depend on openssl-1.0. Other OpenSSL 1.0 depending packages are now being [rebuilt](https://www.archlinux.org/todo/openssl-10-take-3/) to stay compatible with Debian Stable and non-free software. See this [bug report](https://bugs.archlinux.org/task/53836) for more information.

## Boost 1.64 rebuild

Currently a [rebuild](https://www.archlinux.org/todo/boost-1640/) is underway, will land in [testing] soon (tm).

## [pacman-dev] Repository management discussion

Allan started a
[discussion](https://www.mail-archive.com/pacman-dev@archlinux.org/msg15757.html)
on improving the current repository management tooling in pacman. Feedback and
patches are welcome :)

## GCC 7.1 hits [testing]

GCC 7.1 has landed in [testing], please test it and reports issues!


## Security updates of the week

There are quite a lot of security advisories, you can view them [here](https://security.archlinux.org/advisory).
