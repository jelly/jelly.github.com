---
layout: post
title: "unofficial Archlinux repo"
date: 2013-08-25 18:37
comments: false
categories: [Archlinux, AUR, repo ]
---

After heftig's [repo](http://pkgbuild.com/~heftig/), I started an unoffical package repo too. Which contains the following git version of:

-  [youcompleteme](https://github.com/Valloric/YouCompleteMe)
- [mpv](https://github.com/mpv-player/mpv)
- [mutt-kz](https://github.com/karelzak/mutt-kz)

You can add the repo by editing pacman.conf and adding:
{% highlight bash %}
[jelle]
SigLevel = Optional
Server = http://pkgbuild.com/~jelle/repo/$arch
{% endhighlight %}
