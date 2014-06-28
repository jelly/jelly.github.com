---
layout: post
title: "python-zarafa monthly update June"
date: 2014-06-29 20:00
comments: false
categories: [Zarafa, Python, update, June]
---

It's been almost a month since [python-zarafa](https://github.com/zarafagroupware/python-zarafa) has been released on Github. Although the initial commit already contained a lot of features, we added a few nice new features in the month June.  
In this post I will describe the new features we added and I will hopefully provide a new post about improvements on a monthly basis.  

Maildir
-------
The first new feature is maildir support for folders and was added 20 days ago in this [commit](https://github.com/zarafagroupware/python-zarafa/commit/2aaf7faa775e443cd94e986ea8ad3916cf849f4d). 

{% highlight python %}
import zarafa
user = zarafa.Server().user('username')
# Export inbox to maildir in current working directory
user.store.inbox.maildir() 

# Import maildir to inbox
user.store.inbox.read_maildir('Maildir')
{% endhighlight %}

Named properties
----------------
The second new feature is support for named properties, named properties are described in [this post](http://blogs.technet.com/b/exchange/archive/2009/04/06/3407221.aspx) on Microsoft's technet. These properties are for example set on mail messages:

* x-viruschecked
* x-env-sender

These properties now show up in item.properties().

Quota support
-------------
The third feature is support for quotas, Zarafa is capable of setting quota's (hard/soft/warning) globally, per company and per user. For example:

{% highlight python %}
import zarafa

# User quota
user = zarafa.Server().user('username')
print "User store size ",user.store.size
print "User quota warning limit ",user.quota.warn_limit
print "User quota soft warning limit ",user.quota.soft_limit
print "User quota hard warning limit ",user.quota.hard_limit

# Company quota
companies = zarafa.Server().companies()
for company in companies:
	print "Company warning limit ",company.quota.warn_limit
{% endhighlight %}

Address Class
-------------
The last big feature adds an Address class which contains information about for example the sender of an email.
{% highlight python %}
import zarafa
user = zarafa.Server().user('username')
for mail in user.store.inbox.items():
	print "Sender of email " mail.sender.name
	print "From email " mail.sender.email
{% endhighlight %}

Smaller changes are:

* support for http:// socket
* user.fullname
* support for accessing PR_EC_STATSTABLE's (these tables contain information displayed by zarafa-stats)
* public folder support
* bugfixes

Please note that the API is still a work in progress and a lot of functionality might get renamed and refactored in the future. In the future we hope to eventually provide a stable API and online documentation.  
If there are any questions, feel free to contact me via email!
