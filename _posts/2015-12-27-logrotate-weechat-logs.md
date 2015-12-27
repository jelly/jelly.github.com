---
layout: post
title: "Logrotate Weechat logs"
date: 2015-12-27 20:45
comments: false
categories: [Archlinux, Systemd, Weechat, logs]
---

Today I noticed my Weechat logs have grown a lot! ~/.weechat/logs has grown to 360M.
Obviously I need some sort of automation to take care of these logs, enter logrotate.
Luckily [Earnestly](https://github.com/Earnestly) showed me how to use logrotate on it's own some time ago.
So all I what was needed was a logrotate config file (similiar to /etc/logrotate.d/\*) and a systemd user service file.
First off the *~/.local/share/logrotate.d/irc*.

{% highlight bash %}
# weechat logrorate file
compress
missingok
notifempty
copytruncate

# Log files.
"~/.weechat/logs/**log" {}
{% endhighlight %}

The systemd service file located in *~/.config/systemd/user/weechat-rotate.service*.
{% highlight bash %}
[Unit]
Description=Logrotate weechat's logs

[Service]
Type=simple
ExecStart=/usr/bin/logrotate -s %t/logrotate.state  %h/.local/share/logrotate.d/irc -v

[Install]
WantedBy=default.target
{% endhighlight %}

Logrotate will run when Systemd reaches *default.target*, more info about *default.target* can be found [here](https://wiki.archlinux.org/index.php/Systemd#Change_default_target_to_boot_into). You could also use a [Systemd timer](http://www.freedesktop.org/software/systemd/man/systemd.timer.html) to schedule the logrotate weekly or monthly.
To view log output use journalctl.
{% highlight bash %}
journalctl --user -f -u weechat-rotate
{% endhighlight %}
