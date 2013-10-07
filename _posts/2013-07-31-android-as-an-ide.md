---
layout: post
title:  "Android as an IDE"
date:   2013-07-31 19:55:05
description: "Tutorial about howto turn your android device in a efficent working ide with ssh"
categories: ['linux','android']
image: "androidide.png"
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

If you're not, it's time for a ROM change. Go get yourself a [cyanogenmod][cyanogen] the howto
install, once you choose the ROM, is stright-forward (just don't forget to also download the gapps binaries)
By the way cyanogen comes with preinstall  ssh client and Android Terminal Emulator.
It will saves you time.

Or you can read [this excellent guide][guide_root_galaxytab] (for Galaxy Tab 10.1 p7510 owners only)

### Yes, you need a bluetooth keyboard

I tried without it, you're twice as slow and even with [a good software keyboard][hacker_keyboard] you will suffer [RSI][rsi]
So get buy yourself [a bluetooth keyboard][bluetooth_keyboard] and you will certainly need to
tweak its mappings. I write a complete tutorial on [android keyboard mapping] for you.

## Tools

### Life changing

* [Hacker's Keyboard][hacker_keyboard] a soft keyboard with all the keys we, developpers, needs
* [Android Terminal Emulator][android_terminal_emulator] The only free terminal emulator I found on android supporting multiple tabs AND 256 colos
* [BusyBox][busy_box] My favorites unix tools back on android (grep, make, sed, awk,...)
* [SSHDroid][ssh_droid] Allow you to connect to your android device from your computer

I strongly suggest you to install these 4, we will need them all to setup our ssh client
connection.

### Interesting

* [BotBrew][bot_brew] An more complete alternative to busy box (comes with languages such as python or ruby) in fact the latest version allows to install a minimal debian and grant access to apt-get for extanding it. Sadly, I didn't manage to get it work (I didn't try really hard)
* [Vim for android][vim_android] Vim for android
* [Terminal IDE][terminal_ide] A complete java command line java dev environnement (don't know if it's customizable)
* [AIDE][aide] The most convincting java IDE on android, it doesn't convict me a lot.

## Disappoinement

It turns out that there is no /home on android, so you can't store your configuration files
effectively without having to pass them in CLI parameters.

Good news is that [Android Terminal Emulator][android_terminal_emulator] allow you to specify a
`/home` folder. Bad news is that home folder is not taken into account by some binaries (ex: ssh).

I finally gave up on developping directly on android and prefer to ssh on a remote server (my
desktop computer or a remote server).

If I hadn't one under hand I think that I had finally installed a linux in a chroot on top on my
android and accessed it with ssh/Android Terminal Emulator. Or maybe I had tried harder to make
 [BotBrew][bot_brew] work.

Anyway...

# SSH is the answer (at least for me)

But wait...

SSH client is not provided by android nor by busybox.
How do I ssh to my server ?

There are many techniques including:

1. Using a cynaogenmmod rom which comes with a pre-install ssh client
1. using connect bot and be stuck with one only terminal in 8-colors
1. paying for some app that add a, btw, free ssh client to your android
1. hijacking another app ssh client for free

I will detail the last option here.

* Install [SSHDroid][ssh_droid] and start it
* remount your `/system` in `rw` mode:
{% highlight sh linenos=table %}
su
mount -o rw,remount -t yaffs2 /dev/block/mtdblock3 /system
{% endhighlight %}

* find ssh client executable

{% highlight sh linenos=table %}su
find / --name "ssh"
{% endhighlight %}

* link this executable path in your `/system/bin`

{% highlight sh linenos=table %}
ln -s /path/you/find/with/the/find/cli/ssh /system/bin/ssh
{% endhighlight %}
{% highlight sh linenos=table %}
# in my case:
ln -s /data/data/berserker.android.apps.sshdroid/dropbear/ssh /system/bin/ssh
{% endhighlight %}

* check the right on you ssh link, normal users should be able to use it.
I had to grant other user to access directories all the way down to
`/data/data/berserker.android.apps.sshdroid/dropbear/ssh` by adding 5 rights to them or o+rx.
(If you are not familiar with Unix permission  management go and [learn the chmod command
line][chmod].)
* A one point, you will need to generate ssh keys if you wan't a passwordless ssh connection to your server from your android device:


{% highlight sh linenos=table %}
cd /data/data/berserker.android.apps.sshdroid/dropbear/
./dropbearkey -t rsa -f mykey > mykey.pub
{% endhighlight %}

* Your private key is `mykey`: put it in your home directory.
* Your public is `mykey.pub`: put it in your server  `~/.ssh/authorized_keys` file.
* By the way, add your server ip in `/etc/host`

Because there is no `/home` there is no  `.ssh` directory where to store your keys.
As you don't wan't to pass it to the `ssh` command each time, consider create a shell script in
your `/system/bin` or, as I do, add the following command to be executed when you launch [Android Terminal Emulator][android_terminal_emulator]

{% highlight sh linenos=table %}
ssh user@server -i /sdcard/home/id_rsa_db
{% endhighlight %}

Done !

# Conlusion

Combing a great terminal manager like [Android Terminal Emulator][android_terminal_emulator]
with [BusyBox][busy_box] and binaries from [SSHDroid][ssh_droid] allows to access a remote
server in a very convenient way.

Working on the server gives the  power of a real linux developpement environnement and allows to work in a [Unix as an IDE][unix_ide] way from the tablet.

The last thing that makes it possible to work on a tablet is the bluetooth keyboard without it
productivity drops nastily !

I hope this will help you, drop me a comment if you find it usefull.


[ssh_droid]: https://play.google.com/store/apps/details?id=berserker.android.apps.sshdroid "SSHDroid"
[android_terminal_emulator]: https://play.google.com/store/apps/details?id=jackpal.androidterm "Android Terinal emulator"
[busy_box]: https://play.google.com/store/apps/details?id=stericson.busybox "Busy Box for android"
[bot_brew]: http://botbrew.com/ "Bot Brew an alternative to BusyBox"
[vim_android]: https://play.google.com/store/apps/details?id=net.momodalo.app.vimtouch&hl=fr "Vim for android"
[hacker_keyboard]: http://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard "Android Hacker Keyboard"
[android_keyboard_mapping]: {% post_url 2013-07-14-android-keyboard-layout-logitech-tablet-keyboard %} "Customize a bluetooth keyboard layout on android"
[rsi]: https://en.wikipedia.org/wiki/Repetitive_strain_injury
[cyanogen]: http://www.cyanogenmod.org/ "Cyanogenmod for android"
[bluetooth_keyboard]: http://www.logitech.com/en-us/product/tablet-keyboard-android-win8-rt "I like this one"
[guide_root_galaxytab]: http://forum.xda-developers.com/showthread.php?t=2029664 "How to root a stock galaxy tab 10.1"
[terminal_ide]: https://play.google.com/store/apps/details?id=com.spartacusrex.spartacuside "Terminal IDE"
[aide]: https://play.google.com/store/apps/details?id=com.aide.ui "AIDE Android IDE"
[chmod]: http://www.tldp.org/LDP/intro-linux/html/sect_03_04.html "Unix permissions"
[unix_ide]: http://blog.sanctum.geek.nz/series/unix-as-ide/ "Unix as an IDE"
