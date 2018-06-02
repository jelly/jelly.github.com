---
layout: post
title: "Arch monthly May"
date: 2018-06-02 21:37
comments: false
categories: [Arch, monthly, May, news]
---

## Pacman release

Finally! A new pacman release, this version adds some critical bits for reproducible builds and the pacman repository has been shed of misc tools which are now in pacman-contrib. More details in the [changelog](https://git.archlinux.org/pacman.git/commit/?id=64e3a462c61ec620e1a2d86825aae30efd32d6b4) and on [reddit](https://www.reddit.com/r/archlinux/comments/8mu6de/notice_pacman_and_pacmancontrib_split/)

## BUILDINFO Rebuild

For reproducible builds, every package in the repository build on a users system should create exactly the same package as the repository package. To be able to achieve this the packages which where installed in build chroot are recorded in a BUILDINFO file which is added in the .pkg.tar.xz package. BUILDINFO files where added a while ago in pacman, but not every package contains them yet! Interestingly enough even a rolling release distro contains packages from [2013](https://www.archlinux.org/packages/?sort=last_update), these are now being rebuild! This also ties in to the [cleanup of archive.archlinux.org](https://lists.archlinux.org/pipermail/arch-dev-public/2018-May/029261.html), since the archive server is almost full and the 2013/2014/2015 directories will be removed. If you have a good network connection and want to mirror the archive, reach out!

## pkgconf replaces pkg-config

As can be read on the [mailing list](https://lists.archlinux.org/pipermail/arch-dev-public/2018-May/029252.html), pkgconf has now replaced pkg-config.

## GCC 8 in [core]

The latest version of GCC 8 lands in [core], this enables more warnings by default so older packages might fail to build if they enable -Werror.
