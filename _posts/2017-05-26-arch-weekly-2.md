---
layout: post
title: "Arch weekly #2"
date: 2017-05-26 11:00
comments: false
categories: [Arch, weekly, news]
---

This is the second edition of Arch weekly, a small weekly post about the news in
the Arch Linux community.

## Official docker image for Arch Linux!

After [reporting](http://vdwaa.nl/arch/weekly/news/arch-weekly-1/) about the Arch-boxes project last week. Pierres created the [Arch Linux organization on Docker](https://hub.docker.com/u/archlinux/) and created a [base image](https://hub.docker.com/r/archlinux/base/). The docker build script can be found [here](https://github.com/pierres/archlinux-docker). Now you can easily run Arch in docker with a base (regularly updated) image!

{% highlight bash %}
docker run -ti archlinux/base /bin/bash
{% endhighlight %}

## pyalpm 0.8.1 release

A [bugfix release](https://git.archlinux.org/pyalpm.git/tag/?h=0.8.1) for
pyalpm, has been made it fixes one memory leak, removes some unused code and
contains some build fixes.

## Archweb upgrade

Archweb has been [upgraded to 1.8 LTS](https://lists.archlinux.org/pipermail/arch-dev-public/2017-May/028840.html),
previously it was running on 1.7 which is no longer supported. If you encounter
any issues on Archweb please report them on the
[bugtracker](https://bugs.archlinux.org/).

## MariaDB upgrade important news

There are plans to update MariaDB to 10.2.6, this will change the library soname from `libmysqlclient.so` to `libmariadb.so` and some dependency changes, more details are in the [link](https://lists.archlinux.org/pipermail/arch-dev-public/2017-May/028849.html).

## New Trusted User foxxx0

Thore Bödecker joins the TU team, you can read his application [here](https://lists.archlinux.org/pipermail/aur-general/2017-May/033331.html).

## Discussion about improving the overall experience of contributors

Bartłomiej has started a [discussion](https://lists.archlinux.org/pipermail/arch-dev-public/2017-May/028851.html) on arch-dev-public about
improving and getting more external contributors involved in Arch Linux. Not only could existing Arch projects such as pyalpm, archweb and namcap use more contributors for development of new features and fixing bugs. Arch could also use more contributors for new projects and ideas such as rebuild automation and the maintenance of our infrastructure. For those wondering what the infrastructure is about, Arch has a few dedicated servers for the forums, building packages, etc. all these servers are managed with ansible with the [playbooks in git](https://git.archlinux.org/infrastructure.git/)

## Security updates of the week

The following packages received security updates:

* [lynius - arbitrary file overwrite - ASA-201705-20](https://security.archlinux.org/ASA-201705-20)
* [fop - xml external entity injection - ASA-201705-19](https://security.archlinux.org/ASA-201705-19)
* [libplist - multiple issues - ASA-201705-18](https://security.archlinux.org/ASA-201705-18)
* [lxc - insufficient validation - ASA-201705-17](https://security.archlinux.org/ASA-201705-17)
* [openvpn - denial of service - ASA-201705-16](https://security.archlinux.org/ASA-201705-16)
