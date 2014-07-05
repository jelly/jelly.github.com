---
layout: post
title: "Urlwatch systemd timer setup"
date: 2014-06-25 13:37
comments: false
categories: [Archlinux, AUR, Systemd, urlwatch, packaging]
---

As packager for a distro it's useful to get emails when a package you maintain has released a new version. For [Arch Linux](https://www.archlinux.org) users can report that a package is out of date, but unpopular packages might not get flagged by a user automatically.
To solve this issue, I installed [urlwatch](http://thp.io/2008/urlwatch/) which is a program that diffs urls and emails you a diff of the changes.
After you've installed urlwatch, the first launch creates an urlwatch directory in your home directory. The file ~/.urlwatch/urls.txt should contain the urls you want to monitor for changes, for example the list below:
{% highlight bash %}
http://filezilla-project.org/download.php?type=client
http://www.fauskes.net/code/dot2tex/
http://calibre-ebook.com/download_linux
http://www.coker.com.au/bonnie++/
http://users.eastlink.ca/~doiron/bin2iso/
http://www.nongnu.org/autocutsel/
http://xmonad.org/download.html
http://hackage.haskell.org/package/xmobar
http://repository.spotify.com/pool/non-free/s/spotify/
{% endhighlight %}

To make urlwatch check the url list every hour we can create a simple systemd timer unit and systemd service.
{% highlight bash %}
[Unit]
Description=Daily urlwatch update

[Timer]
OnCalendar=hourly

[Install]
WantedBy=multi-user.target
{% endhighlight %}

The systemd service just calls urlwatch with paramters for email delivery. Note that you have to configure the user in the [Service] which runs urlwatch.
{% highlight bash %}
[Unit]
Description=Urlwatch

[Service]
User=youruser
Nice=19
IOSchedulingClass=2
IOSchedulingPriority=7
ExecStart=/usr/bin/urlwatch -t to@mydomain.com -f from@mydomain.com  -s smtpserver
{% endhighlight %}

The .timer and .service files should be saved in /etc/systemd/system as urlwatch.service and urlwatch.timer. Only the timer has to be enabled and started using the following commands:
{% highlight bash %}
systemctl enable urlwatch.timer
systemctl start urlwatch.timer
{% endhighlight %}
