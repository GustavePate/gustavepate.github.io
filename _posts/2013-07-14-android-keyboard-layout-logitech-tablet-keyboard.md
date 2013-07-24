---
layout: post
title:  "Tutorial: A custom android layout for logitech tablet keyboard"
published: true
description: "In this tutorial, we will see howto make a custom layout for an android bluetooth keyboard and by the way, i will give you the logitech tablet keyboard layout to download"
categories: [android]
tags: [logitech, tablet, keyboard, layout, android, howto, tutorial]
image:  false
article: yes
---

* my toc
{:toc}

# context

>We learn from our mistakes.  
    

A few weeks ago, I ordered a logitech tablet keyboard and receive a <span class="big"> logitech tablet keyboard</span> __<small>for ipad</small>__ instead of a <span class="big">logitech tablet keyboard</span> __<small>for android</small>__.

While testing it, i discovered that the keyboard received was an azerty keyboard (like i wanted to) but was recognized as a qwerty keyboard. Looking at it closely, I realized that the place of some keys on this keyboard was not matching the standard place for an azerty keyboard.

I had to remap the entire keyboard :(

# warning

I managed to map correctly my keyboard but it works only with the [hacker keyboard][hacker_keyboard] enabled.
Which is annoying because when you activate a bluetooth keyboard, android automatically switch
on the android legacy software keyboard.

This tutorial involve (optionnaly) a few sticker too, if you don't mind a DIY look for you keyboard.

# prepare yourself

In order to remap a keyboard you need to prepare a few things:
* [Download a keyboard scancode tester][keyboard_test_apk]
* Get some paper (wider than your keyboard) and a paper pencil
* Get some sticker you can cut and write on it to replace mis-placed keys
* Download the [hacker keyboard][hacker_keyboard]
* Get yourself a mean to access your android shell and edit files

I recommand you:
* [Terminal Emulator][terminal_emulator]: 256 colors compatible and allow to open multiple terminals at once
* [Busybox][busy_box] to have a descent text editor (vi)

# let's understand what we will do

1. each physical key owns a unique scan code
1. to this scan code we will assign an android identifier: the key code
1. then we will assign a behaviour to each of these key code (what to print when the key is press with
crtl, shift...) 

For each of theses three steps we will use:
1. [the scan code tester][keyboard_test_apk] and a paper blueprint
1. a file like the `system/usr/keylayout/Generic.kl` file. It associate a scan code to an android keycode
1. a file like the `/system/usr/keychars/Generic.kcm` file. It associate an android key code to a behaviour.

We will see the accepted values for theses two files later in the article.

A keylayout (kl) file example:
{% highlight text linenos=table %}...
key 48    B
key 49    N
key 50    M
key 51    COMMA
key 52    PERIOD
key 53    SLASH
key 54    SHIFT_RIGHT
key 55    NUMPAD_MULTIPLY
key 56    ALT_LEFT
key 57    SPACE
...
{% endhighlight %}

A keychars (kcm) file example:
{% highlight text linenos=table %}...
key 8 {
    label, number:      '8'
    base:               '8'
    shift:              '*'
    ctrl, alt, meta:    none
}

key 9 {
    label, number:      '9'
    base:               '9'
    shift:              '('
    ctrl, alt, meta:    none
}

key SPACE {
    label:              ' '
    base:               ' '
    ctrl:               none
    alt, meta:          fallback SEARCH
}

key ENTER {
    label:              '\n'
    base:               '\n'
    ctrl, alt, meta:    none
}

key TAB {
    label:              '\t'
    base:               '\t'
    ctrl, alt, meta:    none
}
...
{% endhighlight %}

With this two file examples you have the whole layout for the SPACE key.

Understood ? quite simple. The problem  is there are plenty of keys on your keyboard and the
scan codes are not really easy to  memorise. 

# creating a custom keylayout

## The paper blueprint

First, take your keyboard, the sheet of paper and your paper pencil.
You will draw a blueprint of your keyboard that will make your life really easier for the rest
of this tutorial.

When you have a "beautifull" drawing of your keyboard:
1. power on and associate your bluetooth keyboard to your android device
1. switch the input method on the [hacker keyboard][hacker_keyboard] (on my device staying on
the legacy android soft keyboard produced no scancodes)
1. open the [keyboard scan code tester][keyboard_test_apk]
1. for each physical key you want to change the behaviour, take note of the scan code on your
paper blue print.

Now you have something like this:

![my keyboard blueprint][keyboard_blue_print_png]


# The mapping itself

## create the keychars and keylayout files

First, one word of advice: __never ever edit Generic files, never!__ 

Generic files are the last fallback when everything is broken, if you break them you have no fallback left.

We will need to create our custom files, for this we have to know specific information about
your bluetooth keyboard. 

For this log in to a terminal on your android device and type:

{% highlight sh linenos=table %}cat /proc/bus/input/devices
{% endhighlight %}

you shoud see something like this:


{% highlight text linenos=table %}
I: Bus=0005 Vendor=046d Product=b30a Version=011b
N: Name="Logitech Tablet Keyboard IPD"
P: Phys=18:46:17:E0:D6:FF
S: Sysfs=/devices/platform/tegra_uart.2/tty/ttyHS2/hci0/hci0:11/input8
{% endhighlight %}

Write down somewhere the Vendor and Product ids.

In my case:
* Vendor id: 046d
* Product id: b30a

Then, __you absolutely need to be root to do this__ we have to mount the `/system` mount path to
enable read/write operations:

{% highlight sh linenos=table %}su
mount -o rw,remount -t yaffs2 /dev/block/mtdblock3 /system
{% endhighlight %}

Then we will copy the generic files to our custom ones:

{% highlight sh linenos=table %}cd /system/usr/keylayout
cp Generic.kl Vendor_VVVV_Product_PPPP.kl
{% endhighlight %}

where VVVV is the Vendor id and PPPP the product id.

For exemple:

{% highlight sh linenos=table %}cp Generic.kl Vendor_046d_Product_b30a.kl
{% endhighlight %}


Same thing for the keychars file:

{% highlight sh linenos=table %}cd /system/usr/keychars
cp Generic.kcm Vendor_VVVV_Product_PPPP.kcm
{% endhighlight %}

## Edit the keychars and keylayout files

__Warning:__ If one of your file contains error the default android fallback is to switch to the
Generic file. Which makes debugging very difficult. I strongly advise you to make simple change
one key at a time, and test (see below) this change before switching to another key.


First, edit the keylayout .kl file.
For every physical key you want to change, assign an
android keycode to the physical key scancode.

Example: I assign __my__  `+/= ` key to the android `EQUALS` keycode.

{% highlight text linenos=table %}key 13    EQUALS
{% endhighlight %}


Here is the list of [android keycodes][android_keycode_list].
Just remove the **KEYWORD_** before the constant names when using them in the kcm and kl
files.

Here you'll find [the complete keyayout android doc][more_keylayout] 

__Warning:__ If you assign two scan codes to the same android keycode, the file will error.

Then, edit the keychars mapping file .kcm.
The synthax is straight-forward:

{% highlight text linenos=table %}
key EQUALS {
    label, number:      '='
    base:               '='
    shift:              '+'
    ctrl, alt, meta:    '}'
}
{% endhighlight %}

Example: Here I specify that the EQUALS keycode (previously assigned to the +/= key) should
print:
- `=` base, the default value
- `+` when shift is activated
- `}` when crtl or alt or meta is activated
- label,number: just a label, i put the base value here

Here you'll find [the complete keychars android doc][more_keychars] 

Then this is [the unicode characters listing][more_unicode].

# Testing

The only way i found to test this, is to:   

1. power off the bluetooth keyboard
1. desactivate bluetooth on the device
1. reactivate bluetooth on the device
1. power on the bluetooth keyboard
1. select the hacker keyboard because android force me to use the legacy soft keyboard on
bluetooth connection
1. Finally test my key in a text editor

Pffff, that's long, repetitive and boring...

If you have an easier method, write me a comment ;)

# When everything is ok

Don't forget to remount your /system mount point in read-only
{% highlight text linenos=table %}mount -o ro,remount -t yaffs2 /dev/block/mtdblock3 /system
{% endhighlight %}

If you totally remap some keys, now your physical key has nothing more to do with the
result it prints when pressed. It means that's scissors and sticker time !

Look at my beautifull logitech bluetooth tablet keyboard __<small>for ipad</small>__ on
android:

![my keyboard picture][keyboard_picture_png]

Ok, now, you're done !
I hope this article has been helpfull, let me know in the comments.


__By the way:__

Remember to save you keyboard layout and keycode mapping files somewhere.
You don't want to loose this hard work do you ?


If you want, you can send me your own bluetooth keyboard mapping files.
I will publish them here in order to 'centralize' this mess that are keyboard layouts on
android :)


# Downloads

Here you will find links to the kcm and kl files for the logitech tablet keyboard for ipad:

<a class="download_link" href="/ressources/downloads/Vendor_046d_Product_b30a.kl">Vendor_046d_Product_b30a.kl</a>
   

<a class="download_link" href="/ressources/downloads/Vendor_046d_Product_b30a.kcm">Vendor_046d_Product_b30a.kcm</a>

[more_keylayout]: http://source.android.com/devices/tech/input/key-layout-files.html "android documentation for keylayout files"
[more_unicode]: http://en.wikipedia.org/wiki/List_of_Unicode_characters "wikipedia unicode listing"
[more_keychars]: http://source.android.com/devices/tech/input/key-character-map-files.html "android documentation for keychars files"
[android_keycode_list]: http://developer.android.com/reference/android/view/KeyEvent.html "android keycode list"
[keyboard_test_apk]: http://m.tuxli.store.aptoide.com/app/market/name.boyle.chris.keytest/1/278797/Key%20Test "KeyTest apk"
[hacker_keyboard]: https://play.google.com/store/apps/details?id=org.pocketworkstation.pckeyboard "Hacker keyboard"
[terminal_emulator]: https://play.google.com/store/apps/details?id=jackpal.androidterm "Terminal Emulator" 
[busy_box]: https://play.google.com/store/apps/details?id=stericson.busybox "Busy box"
[keyboard_blue_print_png]: /ressources/images/keyboard_layout.png "My keyboard keylayout blueprint"
[keyboard_picture_png]: /ressources/images/logitech_tablet_keyboard.png "My logitech tablet keyboard for android" 
