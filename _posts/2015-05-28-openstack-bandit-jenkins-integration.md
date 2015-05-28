---
layout: post
title: "Openstack Bandit Jenkins integration"
date: 2015-05-28 20:00
comments: false
categories: [Bandit, Python, Security, Jenkins]
---

Some time ago I stumbled on [Bandit](https://wiki.openstack.org/wiki/Security/Projects/Bandit), while I was doing research at work for an automated security linter. Bandit is a tool designed to find common security issues in Python code, which actually found some issues in our code. I was eager to set this up in our Jenkins enviroment when I discovered that Bandit does not support the specific XML output which Jenkins requires (Xunit output to be precise). After a few nights of hacking, the following [commit](https://github.com/stackforge/bandit/commit/99e4d98201e3235f55aa39165e4c3bca8f9d1cd8) added XML output to Bandit.  
Here is a short tutorial on setting up Bandit in Jenkins.

Setup
-----
Install Bandit using the instructions in the [README](http://git.openstack.org/cgit/stackforge/bandit/plain/README.md), note that you will need the Git version for XML output support.  
Now create a new Jenkins job and setup a source code management to fetch your code, then add a new build step like the image below. Where for example *app* is the directory which contains your code.


![Execute shell](https://dedi.vdwaa.nl/jenkins_bandit1.png)

Now add a 'post-build action' to parse the XML output from Bandit and publish the results.

![Execute shell](https://dedi.vdwaa.nl/jenkins_bandit2.png)

Once this is finished, you can trigger and build and you should see tests results as for example shown below.

![Execute shell](https://dedi.vdwaa.nl/jenkins_bandit3.png)

When you click on a failed tests it shows more details about the possible issue. 

![Execute shell](https://dedi.vdwaa.nl/jenkins_bandit4.png)
