---
layout: post
title: "Using python-Zarafa in Zarafa-dagent/Zarafa-spooler plugins"
date: 2015-05-25
comments: false
share: false
categories: [Zarafa, Python, Plugins]
---

Creating plugins for the Zarafa Dagent and Spooler has been quiet complicated since it requires a lot of low-level MAPI knowledge. Since a year ago, we have been focussing on our new high-level Python API called Python-Zarafa, so it made sense to be able to use our preferred API in Spooler and Dagent plugins. 
The current version on [Github](https://github.com/Zarafagroupware/python-Zarafa) made this functionality possible, with a few changes you can now use Python-Zarafa objects such as the Store, Folder and Item in your plugin.

{% highlight python %}
import Zarafa

class MyPlugin(IMapiDAgentPlugin):
    def PostConverting(self, session, addrbook, store, folder, message):
	server = Zarafa.Server(mapisession=session)
	if store:
		store = Zarafa.Store(server, mapiobj=store)
	if folder:
		folder = Zarafa.Folder(store, mapiobj=folder)
	item = Zarafa.Item(mapiobj=message)
	# My plugin logic
{% endhighlight %}

The [example censorship plugin](https://github.com/Zarafagroupware/Zarafa-tools/blob/master/plugins/censorship.py) can now be rewritten to the following code.

{% highlight python %}
import os
import re

from plugintemplates import IMapiDAgentPlugin

import Zarafa

CENSOR = '*censored*'

class CensorShip(IMapiDAgentPlugin):
    def PostConverting(self, session, addrbook, store, folder, message):
        server = Zarafa.Server(mapisession=session)
        if store:
            store = Zarafa.Store(server, mapiobj=store)
        if folder:
            folder = Zarafa.Folder(store, mapiobj=folder)
        item = Zarafa.Item(mapiobj=message)
        body = item.body.text

        if os.path.isfile('/etc/Zarafa/censorship.txt'):
            badwords = [line.strip() for line in file('/etc/Zarafa/censorship.txt')]
            body = re.compile('|'.join(map(re.escape, badwords)), re.I).sub(CENSOR, body)
            self.logger.logDebug('%d badword(s) in body' % len(badwords))
            # Also saves body
            item.body = body
        else:
            self.logger.logError("!--- censorship.txt file not found")
{% endhighlight %}

Hopefully this will make it easier to develop dagent/spooler plugins.
