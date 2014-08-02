---
layout: post
title: "python-zarafa monthly update July"
date: 2014-08-01 20:00
comments: false
categories: [Zarafa, Python, update, July]
---

It's been almost a month since my previous [post](http://vdwaa.nl/zarafa/python/update/june/python-zarafa-monthly-update-june/) on the changes in python-zarafa. This month we continued adding new features to python-zarafa, the following git command shows the changes made since the last post.
{% highlight python %}
i[jelle@P9][~/projects/python-zarafa]%git log --since "JUN 29 2014" --until "AUG 1 2014" --pretty=format:"%h %ar : %s"
20a391b 2 days ago : fix partial rename in class Property
35e5c3e 6 days ago : - address pylint warnings - add Server.remove_user - Server.{get_store, get_user, get_company} now return None instead of throwing an exception - added Folder.state by merging with Server.state - rename mapifolder etc. to mapiobj
842b98d 11 days ago : Update README.md
75f9220 2 weeks ago : properties => prop rename
219351f 3 weeks ago : - Add support for unicode user names - Add support for unicode for Server.create_use - Let Store.folders return nothing if there is no IPM Subtree
746949c 3 weeks ago : - fix some issues with unicode company names - recreate exception for single-tenant setup - improve {Server, Company}.create_user
5820b83 3 weeks ago : add example code for handling tables
816982f 3 weeks ago : Fix shebang
6582594 3 weeks ago : - expose associated - reverse-sort Folder.items() on received date by default - added more folder-level tables
e3d7e4f 4 weeks ago : zarafa-stats.py: zarafa-stats in Python except --top support
0598914 4 weeks ago : Remove property tag since the generator accepts arguments
984d58d 4 weeks ago : - new class Table for MAPI tables - refactor delete - Item.tables() for recipeints and attachments - Rename property\_ and properties to prop and props
{% endhighlight %}
In the following chapters we walk through the main new features in python-zarafa.

Table support
-------------
In python-zarafa we added a table class which abstracts MAPI tables. It provids a few methods which makes it easier to display a MAPI table in various formats, for example csv.

{% highlight python %}
for item in zarafa.Server().user('user').store.inbox:
	print item.table(PR_MESSAGE_RECIPIENTS, columns=[PR_EMAIL_ADDRESS, PR_ENTRYID]).csv(delimiter=';')
	print item.table(PR_MESSAGE_ATTACHMENTS).text()
	print item
	for table in item.tables():
		print table
		print table.csv()
{% endhighlight %}

Address class
-------------
The new Address class represents the sender and recipient of a MAPI message.

{% highlight python %}
item = zarafa.Server().user('user').store.inbox.items().next()
print 'from:', item.sender.name, item.sender.email
for r in item.recipients():
	        print 'rec:', r.name, r.email
{% endhighlight %}

Which prints:
{% highlight python %}
from: john@localhost john@localhost
rec: jaap@localhost jaap@localhost
{% endhighlight %}
Associated folder support
-------------------------
An associated folder in MAPI is a "hidden" table of a folder, which is usually used to store configuration messages for example quota information.
In the [Zarafa-Inspector](https://github.com/zarafagroupware/zarafa-inspector) this functionality is used to look into these MAPI objects.
  You can access the associated folder by calling associated method on a folder.

{% highlight python %}
associated = zarafa.Server().user('user').store.inbox.associated
{% endhighlight %}

User creation/removal
---------------------
The API now also supports the addition and removal of users, which is as simple as the code example below.

{% highlight python %}
server = zarafa.Server()
server.create_user('cowboy', fullname='cowboy bebop')
server.remove_user('cowboy')
{% endhighlight %}

Entryid access
--------------
It wasn't possible to use MAPI Object's entryid to access items directly. Previously we had to loop through the entire inbox to access a partiuclar item. We can now directly access the mapi item if we know it's entryid as you can see in the example below.

{% highlight python %}
user = zarafa.Server().user('user')
entryid = user.store.inbox.items().next().entryid
print 'enytryid', entryid
# Access via store
print 'store   ', user.store.item(entryid).entryid
# Access via folder
print 'inbox   ', user.store.inbox.item(entryid).entryid
{% endhighlight %}

Example output
{% highlight bash %}
enytryid 00000000C80AB3E59F3E420D984664AF5049F1A401000000050000002496BD8A547C46B881BFAC8E9392019700000000
store    00000000C80AB3E59F3E420D984664AF5049F1A401000000050000002496BD8A547C46B881BFAC8E9392019700000000
inbox    00000000C80AB3E59F3E420D984664AF5049F1A401000000050000002496BD8A547C46B881BFAC8E9392019700000000
{% endhighlight %}

These where all the main new features, there are also numerous other small changes which I didn't discuss.
