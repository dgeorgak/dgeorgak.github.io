---
title: feroxbuster
date: 2024-05-29
categories: [Tools, feroxbuster]
tags: [hidden directory, scanning, intro, enumeration]     # TAG names should always be lowercase
---

Imagine the following target: https://www.myexampletarget.com.  Now imagine that somewhere in there is an admin page that presents a login screen that is susceptible to SQLi, and as a bonus it showcases the version of the webapp behind it.  The login page's url is https://www.myexampletarget.com/dev/pages/member.admin.  

But there is one caveat.  You do not know about it.  You do not know the url of the admin page, the login page, the fact that a webapp is hidden somewhere in this website.  You only know the target's url.  Also, the page is hidden.  There is no link path leading to the specific page so spidering will not you help here.

Tough?  Well, not so much.  There are tools that help in discovering hidden directories under a subdomain.  Some famous ones are dirbuster, dirb and gobuster.  Another one is feroxbuster, and this is the one that we are going to talk about here. 
Feroxbuster takes in a url target and a wordlist and applies each word in the wordlist as a directory under the specific url.  For every successful directory it finds, it adds another layer of scanning for subdirectories under the directory that was just found.  On top of that it is very customise-able in terms of how deep we want it to scan, if we also need it to scan for files with specific extensions, how many concurrent scans we allow it to run, if it should follow redirects, etc.

Coming back to our example target above, lets assume that we found https://www.myexampletarget.com behind port 8080.  In order to fire up feroxbuster we run the following command: 
`feroxbuster --url http://www.myexampletarget.com:8080 -w directory-wordlist.txt`
`--url` is used to provide the url target.  If needed we can also specify the port by appending `:port#` at the end
`-w` defines the wordlist that we will be using in order to uncover directories

This is the simplest command we can run with feroxbuster.  The good thing is that we can customise it as much as we'd like.
In our case, our customer was explicit about the fact that the directory `/vendor` is out of bounds during our engagement.  
`--dont-scan /vendor` will instruct feroxbuster to not touch the specific directory

What if we want to also keep an eye in case there are any interesting files in any of the discovered directories.
`-x .pdf` will inform us if any pdf files are present within the directories that the tool lists

While feroxbuster is running we receive a lot of `404` responses which clutter up our results.  
`-C 404` can be used to filter out the status code 404
