---
layout: post
status: publish
published: true
title: ! 'My very first Python script : DocFinder'
author: Julien Lengrand-Lambert
author_login: jlengrand
author_email: julien@lengrand.fr
author_url: http://www.lengrand.fr
wordpress_id: 652
wordpress_url: http://www.lengrand.fr/?p=652
date: 2012-06-17 11:32:37.000000000 +02:00
categories:
- misc
- programming
- Python
tags:
- bash
- programmer
- first code
- code
- docfinder
- opensource
comments: []
---
As you may <a title="python articles" href="http://www.lengrand.fr/category/programmaing/python-2/" target="_blank">already have noticed</a>, I am quite found of Python as a programming language.

I fell in love with Python some years ago, but you should know more about this soon :).

As many of Python users, I started with<strong> <a href="http://diveintopython.org/">Dive into Python</a> </strong>(moreover its <a title="plongez au coeur de python" href="http://diveintopython.adrahon.org/" target="_blank">french translation</a>).

Some days ago, I found myself into the very first Python script I ever written (but still use some times :)). I found it funny (even more when you think that It can be reduced to a<a title="one liners bash" href="http://www.bashoneliners.com/" target="_blank"> one-liner</a>), and thought I would share it.

It aims at listing all documents from a given extension in a folder. To use it, simply run

{% highlight bash %}

$ docfinder folder extension

{% endhighlight %}

So here is the code :

{% highlight python %}
#!/usr/bin/python
#_*_ coding: ISO-8859-15 _*_
import os
import sys

def scan_rep(repertoir, extension):
    """scanne le rep courant pour trouver des tex"""
    for racine, reps, fichiers in os.walk(repertoir, topdown=True):
        for fichier in fichiers:
            if fichier.endswith('.%s' % extension):
                nom_complet=os.path.join(racine, fichier)
                print '%s '%\(nom_complet)
if __name__=='__main__':
    scan_rep(sys.argv[1],sys.argv[2])
{% endhighlight %}

Pretty lame, isn't it ? :)

When you find yourself a<strong> <a title="bad programmer" href="https://sites.google.com/site/yacoset/Home/signs-that-you-re-a-bad-programmer" target="_blank">bad programmer</a></strong>, it is always good to look at the past and see that you actually made some progress :).

You might want to check my<strong><a title="facemovie" href="http://jlengrand.github.com/FaceMovie/" target="_blank"> current pet project</a></strong>, it is also all Python :)

Julien
