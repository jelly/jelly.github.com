---
layout: post
title: "python-zarafa monthly update October"
date: 2014-10-06 20:00
comments: false
categories: [Zarafa, Python, update, October]
---

It's been two months since I've written about a [python-zarafa](https://github.com/zarafagroupware/python-zarafa) update. Lately it has been a busy time and there aren't that much changes made to the API.
In this post I will describe the new features we added.

{% highlight python %}
3236e26 2 days ago : Add User.admin which checks if user is admin
34e3e54 2 days ago : Add try/except to last_logon/last_logof since a new user has no logon property
dc3e916 2 days ago : Add Store.last_login/last_logoff
bb3327b 2 days ago : dictionary comprehension is py2.7 only
a3443ab 5 days ago : - lint fixes - table.dict_rows() make an exception for PR_EC_STATSTABLE_SYSTEM New f
older related functions: - folder.copy - folder.delete - folder.movie - folder.submit
9d982c7 8 days ago : Add z-barplot.py to the readme
31a4c23 8 days ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
dc31ea6 8 days ago : Remove whitespace
a0e54ba 8 days ago : Add barplot stastics script
2c3df17 2 weeks ago : Merge pull request #9 from fbartels/patch-1
33e45fe 2 weeks ago : force default encoding
bd71906 3 weeks ago : Add documentation url
703ba1d 3 weeks ago : Add description for zarafa-spamhandler
82d73f0 3 weeks ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
7e216f4 3 weeks ago : - added sort option to z-plot - add README section for z-plot
75935eb 3 weeks ago : Simple program that uses matplotlib to plot graphs about user data usage
22a24e1 5 weeks ago : Update delete_olditems.py
fad089a 5 weeks ago : Add more MAPI defined folders to Store class
351062e 5 weeks ago : Do not look at admin.cfg if ZARAFA_SOCKET is defined
ff83d14 5 weeks ago : Add auth_user and auth_pass options to zarafa.Server()
3beb326 5 weeks ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
4fc55cc 6 weeks ago : - Store.folders(mail=True) to only generate folders which contain mail - fix Item.b
ody bug - Add folder property to Item class
0f1d23d 6 weeks ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
974dca8 6 weeks ago : Tabs to spaces
5606cf5 6 weeks ago : Add initial release of zarafa-spamhandler
5509cda 7 weeks ago : Fix remote=False to actually show results
d625941 7 weeks ago : Add list_sentitems.py
1440ee4 7 weeks ago : Add outbox
1efa35e 7 weeks ago : Add password authentication option
35826f1 7 weeks ago : Remove print
90328b1 7 weeks ago : Merge branch 'master' of github.com:zarafagroupware/python-zarafa
df0dbdb 7 weeks ago : Add description for monitor,tables and zarfa-stats.py
a3229ab 7 weeks ago : Redo command line arguments, add -U and -P for user session support
ac1c13b 8 weeks ago : update README
ab5ed57 8 weeks ago : Bump version to 0.1
daba691 8 weeks ago : Rename doc to docs
a482a3c 8 weeks ago : - Add verbose flag - Rename spam to junk
cb54d78 8 weeks ago : Make code a bit more fancy...
4273d2f 8 weeks ago : Actually restrict to folder given as parameter
6bf66ef 8 weeks ago : Addition of the spam property.
8bd5778 8 weeks ago : Rename email: to item:
d3a4c2e 8 weeks ago : Add fix for items without received time
128907b 8 weeks ago : Rename examples to scripts - Add several new examples replacing the examples.py
329aca8 9 weeks ago : Add README.md
4d186c3 9 weeks ago : Add delete_olditems.py: Allows a system administrator to delete old email for a user. e.g. python delete_olditems.py -u username 30 will delete all mails older than 30 days based on the receive date of the emails.
c6e977a 9 weeks ago : - fix subfolder bug in Folder.folders - make Folder.create_folder return a mapi folder - add store.folder(key)
a790523 9 weeks ago : fix documentation chapter
1ec3062 9 weeks ago : add sphinx documentation part
63d69f7 9 weeks ago : add sphinx Makefile to create documentation
eb303e6 9 weeks ago : - Add docstrings for classes - Add Body class, which can provide an html or plaintext representation of a MAPI message body - add folder.item(entryid)
{% endhighlight %}

Sphinx documentation
--------------------

We've added online [Sphinx documentation](http://doc.zarafa.com/trunk/Python_Zarafa/), which should make it easier to hack on Python-Zarafa. The documentation is still work in progress so expect us to add missing parts and improvements.

User session support
--------------------
The API had initial support for only a SYSTEM session, but now it also supports user sessions. User sessions can be used with the command line switches -U (username) and -P (password).
In a Python program the user session support looks as following.
{% highlight python %}
import zarafa

server = zarafa.Server(auth_user='bob',auth_pass='test')

# Bob's inbox Folder
inbox = server.users().next().store.inbox

{% endhighlight %}
The generator server.users() still returns a generator, but it only contains one User object.

New properties
--------------
User, Store, Folder, etc. have received new exposed MAPI properties, so that you don't need to look up the MAPI property definition.
{% highlight python %}
import zarafa

server = zarafa.Server(auth_user='bob',auth_pass='test')
bob = server.users().next()
store = bob.store
print "last logoff:", store.last_logoff

# More store Folders definitions
print = store.sentmail
print store.junk

# User.admin to verify if a user is an admin
print "Is bob admin? ", bob.admin
{% endhighlight %}

Which prints:
{% highlight python %}
last logoff: 2014-10-04 19:44:39
Folder(Sent Items)
Folder(Junk E-mail)
Is bob admin? False
{% endhighlight %}

Folder related functions
------------------------
There are three new Folder related functions, Folder.copy(), Folder.move() and Folder.delete(). 
{% highlight python %}
import zarafa
store = zarafa.Server(auth_user='bob',auth_pass='test').users().next().store
print list(store.inbox.items())
# Copy all items to the junk folder
store.inbox.copy(store.inbox.items(),store.junk)
print list(store.junk.items())
# Move all items to the junk folder
store.inbox.move(store.inbox.items(),store.junk)
print list(store.inbox.items())
print list(store.junk.items())
# Empty junk
store.junk.delete(store.junk.items())
{% endhighlight %}
Which prints:
{% highlight python %}
# Inbox
[Item(Undelivered Mail Returned to Sender)]
# Junk
[Item(Undelivered Mail Returned to Sender)]
# Inbox
[]
# Junk
[Item(Undelivered Mail Returned to Sender), Item(Undelivered Mail Returned to Sender)]
# Junk
[]
{% endhighlight %}

These functions combined with ICS can for example create a client-side rule.
{% highlight python %}
import zarafa
import time

class importer:
    def __init__(self, folder, target):
        self.folder = folder
        self.target = target

    def update(self, item, flags):
        if 'spam' in item.subject:
            print 'trashing..', item
            self.folder.move(item, self.target)

    def delete(self, item, flags):
        pass

server = zarafa.Server()
store = server.user(server.options.auth_user).store
inbox, junk = store.inbox, store.junk

state = inbox.state
while True:
    state = inbox.sync(importer(inbox, junk), state)
    time.sleep(1)
{% endhighlight %}

These where the most interesting changes in Python-Zarafa from 1 August till 6 October.
