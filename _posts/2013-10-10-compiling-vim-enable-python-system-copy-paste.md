---
layout: post
title:  "Compiling vim with python and system clipboard support on Ubuntu"
date:   2013-10-10 12:23:05
description: "A quick Howto compile vim for python and system clipboard support"
categories: [linux]
article: yes
tags: [vim, python, clipboard, compile, howto]
image: "vimcompile.png"
---

Edit 2014-04-16: added configure option for compilation without x

Here is a quick reminder of how-to compile vim for:
* python support
* ruby support
* x11 copy / paste

It will be install in /usr/local/bin by default.

Don't forget to adjust the values of the configure command for:

- `--with-compiledby`
- `--with-python-config-dir`
- `--with-ruby-command`


### Get dependencies

Run this if you are using Ubuntu otherwise adapt it to your package manager.

{% highlight text linenos=table %}

sudo apt-get install libncurses5-dev libgnome2-dev \
    libgnomeui-dev libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
    libcairo2-dev libx11-dev libxpm-dev libxt-dev libncurses-dev \
    libgnome2-dev vim-gnome libgtk2.0-dev libatk1.0-dev libbonoboui2-dev \
    libcairo2-dev libx11-dev libxpm-dev libxt-dev python python-dev ruby ruby-dev
{% endhighlight %}

### Fetch the source

{% highlight text linenos=table %}
mkdir ~/vim
cd ~/vim
git clone https://github.com/b4winckler/vim
cd vim/src
make distclean
cd ..
{% endhighlight %}

### Configure

{% highlight text linenos=table %}
./configure --with-features=huge --enable-gui=gnome2 --enable-pythoninterp=yes \
    --enable-rubyinterp=yes --enable-cscope --enable-gui=auto \
    --enable-gtk2-check --enable-gnome-check \
    --enable-fail-if-missing --enable-multibyte --enable-fontset \
    --with-x --with-compiledby="GustavePate" \
    --with-python-config-dir=/usr/lib/python2.7/config-x86_64-linux-gnu/ \
    --with-ruby-command=/usr/bin/ruby
{% endhighlight %}

** OR without X support **

{% highlight text linenos=table %}
./configure  --with-features=huge --enable-pythoninterp=yes -enable-rubyinterp=yes  -enable-cscope --enable-fail-if-missing --enable-multibyte
--enable-fontset -with-compiledby="GustavePate" --with-python-config-dir=/usr/lib/python2.7/config/ --with-ruby-command=/usr/bin/ruby --disable-gui
--without-x
{% endhighlight %}

then

{% highlight text linenos=table %}
make
sudo make install
{% endhighlight %}

### Check installation

{% highlight text linenos=table %}
#check if your tailor made vim is called first (should display /usr/local/bin/vim
which vim

#if your /usr/local/bin has not precedence on /usr/bin
sudo update-alternatives --install /usr/bin/vim vim /usr/local/bin/vim
{% endhighlight %}


