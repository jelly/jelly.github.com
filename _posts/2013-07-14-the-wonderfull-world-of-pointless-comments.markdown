---
layout: post
title: "The wonderfull world of pointless comments"
date: 2013-07-14 15:33
comments: false
categories: [libreoffice, programming]
---

So I started working on LibreOffice's [easyhacks](https://wiki.documentfoundation.org/Development/Easy_Hacks_by_required_Skil), there are numerous hacks in different categories. But I quickly discovered I didn't want to dive in the complicated or time consuming bugs. Luckily the easyhacks wiki has a lot of bugs that don't require a lot of knowledge of LibreOffice's internals. 

The first bug I stumbled on was ["remove pointless comments and ASCII art"](https://bugs.freedesktop.org/show_bug.cgi?id=62475), so this seems like an easy bug. The bug is about useless comments in the Libreoffice codebase, an example of this bug can be found [here](http://cgit.freedesktop.org/libreoffice/core/commit/?id=b9008dfbef5c744e351c6bb1edbfc30f364731c8). And I soon discovered that the scale of these comments is mind-boggling as a quick "git grep" is showing.
{% highlight bash %}
git grep -E '\/\/[-=_#\.]{12,}' | wc -l
46482
{% endhighlight %}

With this regex and [sed](http://www.gnu.org/software/sed/manual/sed.html) I could easily remove a lot of these comments as you can see in the code below.
{% highlight bash %}
find moduledirectory -name "*.cxx"  > files 
find moduledirectory -name "*.hxx"  >> files 

sed -r -i '/\/\/[-=_#\.]{12,}/d' `cat files`
{% endhighlight %}
Although I am still surprised that developers created these comments, I'm happy that I've found an easy way to improve readability of LibreOffice's codebase and getting rid of ~ 46000 useless lines.
