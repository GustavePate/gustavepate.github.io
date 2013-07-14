---
layout: post
title:  An android layout for logitech tablet keyboard
published: true
tags: template meaningless
description: "We will see how to make a custom layout for an android bluetooth keyboard and explain  the logitech tablet keyboard layout"
categories: [android]
keywords: [logitech, tablet, keyboard, layout, android, howto]
image:  false
article: yes
---

# context

We often learn from our mistakes. I ordered a logitech tablet keyboard and receive an 
tablet keyboard <small>for ipad</small> instead of an logitech tablet keyboard <small>for
android</small>. While testing it, i discovered that the keyboard received was an azerty
keyboard (like i wanted to) but was recognized as a qwerty keyboard.

And at looking closely to the keyboard I realized that the place of some keys on this keyboard
was not matching the standard place for an azerty keyboard.

I had to remap the entire keyboard :(

# warning

I maanged to map correctly my keyboard but it works only with the [hacker keyboard][hacker_keyboard] enabled.
Which is annoying because when you activate a bluetooth keyboard android automatically switch
on the android legacy keyboard.
My methode involves a few sticker too if you don't mind a diy look for you keyboard.

# procedure

In order to remap a keyboard you need to prepare a few things.
* [Download a keyboard scancode tester][keyboard_test_apk]
* Get some paper (wider than your keyboard) and a paper pencil
* Get some sticker you can cut and write on it to replace mis-placed keys
* Download the [hacker keyboard][hacker_keyboard]
* Get yourself a mean to access your android shell and edit files

I recommand you:
* [Terminal Emulator][terminal_emulator] which is 256 colors compatible and allow to open multiple terminals at once
* [Busybox][busy_box] to have a descent text editor (vi)



[keyboard_test_apk]: 
[hacker_keyboard]: 
[terminal_emulator]:
[busy_box]:
