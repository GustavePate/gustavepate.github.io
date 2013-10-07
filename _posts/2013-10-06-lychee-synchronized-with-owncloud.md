---
layout: post
title:  "Synchronise Lychee with owncloud (or any directory)"
date:   2013-10-06 16:19:05
description: "Howto synchronize Lychee and owncloud, dropbox or any directory"
categories: web
image: false
article: yes
image: "/ressources/images/lycheesync.png"
tags: ['python', 'linux', 'server', 'web', 'lychee', 'owncloud', 'dropbox', 'synchronization']
---

# Context

On one side there's [Lychee][lychee].

Lychee is a marvelous self hosted photo management application. It's very good looking, responsive design enabled and has avery feature you wan't to simply share your private photos. No ad, no I need a `[facebook, twitter, google, whatever]` account to see the photos. Just perfect to share photos with your friends and family with no bullshit.

If you don't know this project just check it out !

On the other side, there's [Owncloud][owncloud].

Owncloud is a fantastic self hosted dropbox replacement and is begining to be well known in the FOSS community. But, there's a problem with this app. The web client Photo management is broken. The web client is far more too complex for my grand mother. Too see a photo, have a slideshow and download a full album you have to use 2 differents apps and go through a whole file tree which is not grand mother compliant.

# So ...

I needed to offer my family and friends a great photo browsing experience. Owncloud photo management has not passed the grandma test but lychee has brilliantly. I don't wan't the hassle to FTP photos on a server, the owncloud way of doing it, drag and drop in a local folder, is just perfect.

So I wrote [lycheesync][lycheesync], a little python program that you can schedule everyday and that enslave lychee with a directory content.
It works fine with owncloud (owncloud photos are stored in a directory) and can work with any directory you can access from your lychee server (dropbox, mega, skydrive, whatever...)

I hope you will enjoy it !

I do ;)

[lychee]: http://lychee.electerious.com/  "lychee, the most gorgeous photo management application on the web"
[owncloud]: http://owncloud.org/ "owncloud, the dropbox replacement that do more than dropbox"
[lycheesync]: https://github.com/GustavePate/lycheesync "lycheesync, the best of owncloud with the best of lychee"
