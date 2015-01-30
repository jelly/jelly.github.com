---
layout: post
title: "python-zarafa monthly update Januari"
modified: 2015-01-30 20:04:55 +0100
comments: false
categories: [Zarafa, Python, update, Januari]
---

It's a new year, so it's also time for a new update about [python-zarafa]((https://github.com/zarafagroupware/python-zarafa). The last update was in [October]({% post_url 2014-10-06-python-zarafa-monthly-update-october %}) and a lot of new improvements and features have been added. In this article I'll walk through the new features and improvements. A detailed git changelog can be found at the end of the article.

Scripts
=======

We added a lot of new example scripts to the [Github repository](https://github.com/zarafagroupware/python-zarafa).

* zarafa-tracer - a script which monitors a MAPI folder for changes and displays the changes in a diff-like format.
* import_ics - import .ics files (calendar)
* urwap - MAPI curses console mail client 
* rule.py - client side rules
* set-out-of-office.py - example script to show how to use the new OutofOffice functionality

OutofOffice
===========

Python-zarafa now has support for setting and getting Outofoffice information.
{% highlight python %}
>>> bob = zarafa.Server().user('bob')
>>> bob.outofoffice.update(enabled=True, message='I am out of office', subject='Out of office')
>>> print 'enabled: %s, message: %s, subject: %s' % (bob.outofoffice.enabled, bob.outofoffice.message, bob.outofoffice.subject)
enabled: True, message: I am out of office, subject: Out of office
# It is also possible to set indiviual properties
>>> bob.outofoffice.enabled = False
>>> bob.outofoffice.subject = 'Hi, I am out of office till 30 October.'
{% endhighlight %}

Quota
=====
Another new feature is Quota support, there was already support for reading Quota information in python-zarafa but now there is partial support for setting user Quota's. Setting Quota's is still work in progress, so expect that it might contain some bugs and feel free to report them to our Github tracker!

{% highlight python %}
>>> bob = zarafa.Server().user('bob')
>>> print zarafa._bytes_to_human(bob.quota.hard_limit)
100 mb
>>> bob.quota.hard_limit = zarafa._human_to_bytes('200 mb')
>>> print zarafa._bytes_to_human(bob.quota.hard_limit)
200 mb
{% endhighlight %}

Creating and sending an email
=============================

This is probably the most interesting feature for python-zarafa, the ability to create an new mail item and sending it. 
{% highlight python %}
>>> mail = bob.outbox.create_item(subject='new mail', to='bob@zarafa.local', body='This is my plaintext body')
>>> print 'subject=%s, to=%s, body=%s'  % (mail.subject, mail.to[0], mail.body.text)
subject=new mail, to=Address(bob@zarafa.local), body=This is my plaintext body
>>> mail.send()
>>> print list(bob.inbox.items())
[Item(new mail)]
{% endhighlight %}

This is also a work in progess feature, since not all of the extra properties can be set yet (importance, etc.) .It should be enough to send an weekly email with for example quota information of all users.

Various fixes
=============

We have done a lot of fixes and further polishing of the API, here is a short list of the interesting improvements

* Add support for the creationg of a server-wide store
* Add folder.empty() which empties a folder 
* Added support for more *shortcuts*; user.tasks, user.journal, user.contacts
* Export folder to maildir with a specific path

Feel free to create a feature request or bugreport in our Github bug tracker.

Git shortlog
============

{% highlight python %}
268d034 8 hours ago : Add description for rule.py and send.pyn-zarafa] 
7a37b71 8 hours ago : Add two new example scripts
5fd46e9 2 days ago : Add simple MAPI console client
8770159 3 days ago : Make the link for documentation bigger
268d034 8 hours ago : Add description for rule.py and send.py
7a37b71 8 hours ago : Add two new example scripts
5fd46e9 2 days ago : Add simple MAPI console client
8770159 3 days ago : Make the link for documentation bigger
4d4265c 3 days ago : Fix readme
8135740 6 days ago : describe quota stuff
2101902 7 days ago : Item.to: support 'name <email>' syntax
0db90b8 8 days ago : When a folder has no PR_DISPLAY_NAME check if it is the root folder else return an empty string
6461287 8 days ago : ignore orig files from patch
261c5ac 10 days ago : fix indent
df682da 10 days ago : update Quota cached values in def update
f0a443e 13 days ago : - Rename Quota._update to Quota.update so you can easily update all quota information
e2b94e7 2 weeks ago : Only support one archiverserver
653ea3c 2 weeks ago : rename Folder.empty(subfolder) arg to recurse for uniformity
3f7f861 2 weeks ago : Cleanup Property class: -self.mapiobj should probably point to SPropValue -overload __lt__ so sorted(props) works.. bit overkill perhaps -reduce arguments which can be easily deduced by Property.__init__ from SPropValue -avoid some local variables
e939d4c 2 weeks ago : Remove old comment
90eae11 3 weeks ago : Add support for setting Quota limits
bb12a2a 3 weeks ago : simplify updating outofoffice
0827a19 3 weeks ago : Example to show how to set Out of Office with python-zarafa
dff11c4 3 weeks ago : Simplify User.__init__ check, update docstrings
ad3796f 3 weeks ago : In zarafa.Server().users() arguments are switched
cb27fc3 3 weeks ago : Create a general _update() function for out of office information, to enable a one line update for oof info
72ada05 3 weeks ago : fix duplicated enties
f47b463 3 weeks ago : Add ability to set/read out of office settings of a uses #6
938ad50 3 weeks ago : Add basic item (de)serialization using pickle
c325e99 3 weeks ago : Add User.outofoffice
507ace3 4 weeks ago : apparently TBL_ALL_COLUMNS does not work for all tables.. so fall-back to flags=0
4bb5792 4 weeks ago : Add shortcut for getting user: zarafa.User(name)
bd05e17 5 weeks ago : Add unicode support to create_folder, add folder.get_folder, add folder.folder(create=True) flag to create non existing folders add folder.folder(name/entryid, recurse=False) to do a non-recursive search)
dc454c9 5 weeks ago :  "Ham" folder to exist in root of mail store.
9365478 5 weeks ago : Add several changes including the ability to change a folders name.
7918523 5 weeks ago : Store.tasks, property is set on root folder not store
05f55b4 5 weeks ago : Add store.journal and store.notes
3199b93 6 weeks ago : Merge pull request #16 from m42e/learnham
fdf2c22 6 weeks ago : Make configurable and autocreate folder
627478e 2 months ago : Added possibility to learn ham
cb27120 6 weeks ago : Merge pull request #12 from Faldon/master
25480b8 6 weeks ago : Made mandatory email property in create_user optional and provide a 'best guess' if not passed
7b568fd 7 weeks ago : Added mandatory email property on creating user. Added method for modifying/updating a user. Added constant INACTIVE_USER for inactive mailusers.
84f6169 7 weeks ago : Add definition for PS_INTERNET_HEADERS
7014793 7 weeks ago : Add support for the creation of a server-wide public store
c8777f4 7 weeks ago : open table with MAPI_UNICODE
fb77282 8 weeks ago : Fix recipienttable, since we don't pass MAPI_UNICODE
b8390da 8 weeks ago : Add optional dep vobject to readme
72d7dbf 9 weeks ago : Add delimiter for path
6353c4e 9 weeks ago : Return None instead of an except in user.archive_servers()
f939e20 9 weeks ago : search folders have no hierachy table, so no subfolders
1f403f5 9 weeks ago : fix import on some RHEL distros
a1704bc 9 weeks ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
cd354e2 9 weeks ago : - Add folder.parent - Refactor folder.root option by using a try/except
3e7f903 10 weeks ago : Give maildir a path option
612c381 10 weeks ago : fix trailing whitespaces
330b464 10 weeks ago : use MAPI_UNICODE due to changes in python-mapi
a31f2ca 10 weeks ago : make Server.stores() return the public store
da5bb36 2 months ago : Use PR_SOURCEKEY to create the mapping so we do not an expensive lookup
e858500 3 months ago : Add import_ics script
e13bee2 3 months ago : Add z-tracer.py
7c2d0dc 3 months ago : Print folder names if move fails.
6496548 3 months ago : Fix order in which True is set.
af4ad91 3 months ago : Fix code styling
7a61122 3 months ago : Add new fix-ipm-subtree.py script, use at your own risk!!!
e332739 3 months ago : Have Store>Folder look at system folders
fcb4ad4 3 months ago : Add core updates
2f419af 3 months ago : Update python-zarafa core, add script remove-calendar-items.py
{% endhighlight %}
