---
layout: post
title: "python-zarafa monthly update July"
date: 2014-08-01 20:00
comments: false
categories: [Zarafa, Python, update, July]
---

It's been almost a month since my previous [post](http://vdwaa.nl/zarafa/python/update/june/python-zarafa-monthly-update-june/) has been released on Github. Since then we haven't stopped with the development of this new api.
The following git command shows the changes made since the last post.
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


Table support
-------------
In python-zarafa we added the a table class which abstrats MAPI tables. It provids a few methods which makes it easier to display a MAPI table in various formats.

{% highlight python %}
for item in zarafa.Server().user('user1').store.inbox:
	print item.table(PR_MESSAGE_RECIPIENTS, columns=[PR_EMAIL_ADDRESS, PR_ENTRYID]).csv(delimiter=';')
	print item.table(PR_MESSAGE_ATTACHMENTS).text()
	print item
	for t in item.tables():
		print t
		print t.csv()
{% endhighlight %}

Address class
-------------
The Address class represents the sender and recipient of a MAPI message. 

{% highlight python %}
import zarafa
server = zarafa.Server()
	for item in server.user('user').store.inbox:
	    print 'from:', item.sender.name, item.sender.email
	        for r in item.recipients():
			        print 'rec:', r.name, r.email

{% endhighlight %}
Associated folder support
-------------------------

{% highlight python %}
{% endhighlight %}

User creation/removal
---------------------

{% highlight python %}
{% endhighlight %}

Entryid access
--------------

{% highlight python %}
eid = z.user('user1').store.inbox.items().next().entryid
print 'eid', eid
print 'sto', z.user('user1').store.item(eid).entryid
print 'via', z.user('user1').store.inbox.item(eid).entryid
{% endhighlight %}
