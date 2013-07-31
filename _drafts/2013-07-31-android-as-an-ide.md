---
layout: post
title:  "Android as an IDE"
date:   2013-07-31 19:55:05
description: "Tutorial about howto turn your android device in a efficent working ide" 
categories: linux,android
image: false
article: yes
tags: ['android','vim','ide','bash','ssh']
---

* my toc
{:toc}

# Context 

As I was suffering from backpain for months, I needed to work on personnal projects without
leaving my bed. Laptop was not an option and I already owned a Samsung Galaxy Tab which turned
into a great developement plateform.

I struggled with a few points so I share these online ;)

# What stands for an IDE

While working on a mobile device you can't afford to use a IDE such as Eclipse which as far too
many buttons to be usable. I don't think it can be possible to port such tools effectivelly on
handheld device. 
In the state of the art android way of doing it, It involves installing next to your android on
your device and communicating from your android OS to your desktop linux programs via VNC.

Too slow, too complicated: not efficient.

So I have to wonder, how did I do before programming in Eclipse ?
Well, I used Emacs.

So let's go for text only IDE !
While searching the web I stumble uppon the "Unix as an IDE" idea. 
Being a great fan of Linux, I had already a lot of the keytools in my skills.

Only, I forgot the "text editor part"... My emacs skills are gone a long while ago and I found
vim to be already ported to android and easier to use in a shell.

# Let's make our IDE

## Prerequisites

### You need to be root, 

If you're not, it's time for a ROM change. Go get yopurself a [cyanogenmod][cyanogen] the howto
install, once you choose the ROM, is stright-forward (just don't forget to also download the gapps binaries) 
By the way cyanogen comes with preinstall  ssh client and Android Terminal Emulator. 
It will saves you time.

Or you can read [this excellent guide][guide_root_galaxytab] (for Galaxy Tab 10.1 p7510 owners only)

### Yes you need a bluetooth keyboard

I tried without it, you're twice as slow and even with [a good software keyboard][hacker_keyboard] you will suffer [RSI][rsi]
So get buy yourself [a bluetooth keyboard][bluetooth_keyboard] and you will certainly need to
tweak its mappings. I write a complete tutorial on [android keyboard mapping] for you.  

## Tools

### Life changing

* [Hacker's Keyboard][hacker_keyboard] a soft keyboard with all the keys we, developpers, needs 
* [Android Terminal Emulator][android_terminal_emulator] The only free terminal emulator I found on android supporting multiple tabs AND 256 colos
* [BusyBox][busy_box] My favorites unix tools back on android (grep, make, sed, awk,...) 
* [SSHDroid][ssh_droid] Allow you to connect to your android device from your computer 

### Intersting

* [BotBrew][bot_brew] An more complete alternative to busy box (comes with languages such as * python or ruby)
* [Vim for android][vim_android] Vim for android

# Disappoinement

It turns out that there is no /home on android, so you can't store your configuration files
effectively without having to pass them in CLI parameters.

I finally gave up on developping directly on android and prefer to ssh on a remote server (my
desktop computer or a remote server).

If I hadn't one under hand I think that I had finally installed a linux in a chroot on top on my
android and accessed it with ssh/Android Terminal Emulator.

Anyway...

# SSH is the answer (at least for me)

But wait...

SSH client is not provided by android nor by busybox nor by botbrew.
How do I ssh to my server ?

There are many techniques including:
1. Using a cynaogenmmod rom which comes with a pre-install ssh client
1. using connect bot and be stuck with one only terminal in 8-colors 
1. paying for some app that add a free ssh client to your android
1. hijacking another app ssh client for free

I will detail the third option here.

* Install [Dropbear][drop_bear] and start it

* remount your `/system` in `rw` mode: 

{% highlight sh linenos=table %}
su
mount -o rw,remount -t yaffs2 /dev/block/mtdblock3 /system
{% endhighlight %}

* find ssh client executable

{% highlight sh linenos=table %}su
find / --name "*ssh*"
{% endhighlight %}

* link this executable path in your `/system/bin`

{% highlight sh linenos=table %}
ln -s /path/you/find/with/the/find/cli/ssh /system/bin/ssh
{% endhighlight %}

* By the way, add your server ip in `/etc/host`

Okay, your big enough to find how to generate ssh keys and it's not the point of this post.
Because there is no `/home` there is no  `.ssh` directory where to store your keys.
As you don't wan't to pass it to the `ssh` command each time, consider create a bash script in
your `/system/bin` or, as I do, add 

{% highlight sh linenos=table %}
ssh user@server -i /sdcard/home/id_rsa_db
{% endhighlight %}

to be executed when you launch [Android Terminal Emulator][android_terminal_emulator]

Ohh !! I just see that you actually *can* specify a `/home` in the new version of Terminal
Emulator but ssh just don't care ;)



# Conlusion

[ssh_droid]: https://play.google.com/store/apps/details?id=berserker.android.apps.sshdroid 
[android_terminal_emulator]: https://play.google.com/store/apps/details?id=jackpal.androidterm
[busy_box]: https://play.google.com/store/apps/details?id=stericson.busybox
[drop_bear]: https://play.google.com/store/apps/details?id=me.shkschneider.dropbearserver
[bot_brew]: http://botbrew.com/ "Bot Brew an alternative to BusyBox"
[vim_android]: https://play.google.com/store/apps/details?id=net.momodalo.app.vimtouch&hl=fr "Vim for android"
[hacker_keyboard]: http://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard 
[android_keyboard_mapping]: {% post_url 2013-07-14-android-keyboard-layout-logitech-tablet-keyboard %}
[rsi]: https://en.wikipedia.org/wiki/Repetitive_strain_injury 
[cyanogen]: http://www.cyanogenmod.org/ 
[bluetooth_keyboard]: http://www.logitech.com/en-us/product/tablet-keyboard-android-win8-rt "I like this one"
[guide_root_galaxytab]: http://forum.xda-developers.com/showthread.php?t=2029664 "Howto root a galaxy tab p7510"


