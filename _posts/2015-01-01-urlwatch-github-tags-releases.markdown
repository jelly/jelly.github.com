---
layout: post
title: "urlwatch github urls"
date: 2015-01-01 20:00
comments: false
categories: [urlwatch, github, python]
---

I've talked about an [urlwatch systemd timer setup](http://vdwaa.nl/archlinux/aur/systemd/urlwatch/packaging/urlwatch-systemd-setup/), but monitoring github pages is troublesome since the HTML data changes per visit.
Luckily urlwatch has support for hooks, which triggers after the data has been retrieved and before the data is compared with the previous retrieved data. Hooks can be created by creating a file in *~/.urlwatch/libs/hooks.py* with the following content.

{% highlight python %}
from BeautifulSoup import BeautifulSoup

def filter(url, data):
	if 'github' in url:
		soup = BeautifulSoup(data)

		if soup.find('span', {'class': 'tag-name'}):
			return str(soup.find('span', {'class': 'tag-name'}))
		elif soup.find('h1', {'class': 'release-title'}):
			return str(soup.find('h1', {'class': 'release-title'}))
		else:
			return data
	else:
		return data
{% endhighlight %}

This filter is targeted at Github release and tags pages like for example: https://github.com/scipy/scipy/releases.
