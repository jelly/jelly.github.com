---
layout: post
title: "Spotify music box"
date: 2016-12-05 20:00
comments: false
categories: [Rust, Spotify, librespot, ARM, Nanopi neo]
---

For some time I've wanted to play Spotify music on my stereo installation, except it doesn't have bluetooth. I do own a nice aarch64 amlogic S905X based media center which runs LibreELEC, except libspotify which I normally use in combination with mopdiy. Libspotify (a binary blob from Spotify(tm)) however does not support aarc64.
So I decided to use one of my spare ARM boards, the nanopi NEO it has USB for the DAC and ethernet for streaming music and it's supported in mailine (ethernet however requires patches) and [librespot](https://github.com/plietar/librespot). Librespot is a service which enables my phone (or any other official spotify-client running device) to play music on the ARM board running librespot. All what was required was compiling librespot (rust program) for ARMv7.

Arch Linux ARM does not offer rust as binary package, so you'd have to use rustup and run the nightly channel because the default rust channel segfaults.

{% highlight bash %}
curl -sSf https://static.rust-lang.org/rustup.sh | sh -s -- --channel=nightly
{% endhighlight %}

Compiling on an ARM board with 512 MB ram and no swap is currently not do-able with rust even with 'cargo build -j1'. So I switched to a beefier board (orange pi pc) with 1GB ram which is luckily enough to compile libreelec.
{% highlight bash %}
pacman -S protobuf portaudio
cargo build --release -j1
{% endhighlight %}

After it was compiled, install the cargo binary with 'cargo install' and copied the systemd unit from the [AUR package](https://aur.archlinux.org/cgit/aur.git/tree/?h=librespot-git). So far librespot works as expected and hasn't crashed while running for a week.

Later I intend to integrate the nanopi and dac in a nice case with a power button to poweroff/poweron the nanopi.
