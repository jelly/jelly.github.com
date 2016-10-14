---
layout: post
title: "Git credential helper pass"
date: 2016-10-14 13:17
comments: false
categories: [pass, Git, linux]
---

At work we use Git with https auth, which sadly means I can't use ssh keys. Since I don't want to enter my password every time I pull or push changes to the server, I wanted to use my password manager to handle this for me.
Git has pluggable credential helper support for gnome-keyring and netrc, adding [pass](https://www.passwordstore.org/) support turned to be quite easy.
Create a script called "pass-git.sh" and put the following contents in it where 'gitpassword' is your password entry.

{% highlight bash %}
#!/bin/bash

echo "password="$(pass show gitpassword)
{% endhighlight %}

In your git directory which uses https auth execute the following command to setup the script as a credential helper.
{% highlight bash %}
git config credential.helper ~/bin/pass-git.sh
{% endhighlight %}

Voila, that's all that's it.
