---
layout: post
title:  "Secure a Linux server"
date:   2013-08-14 19:55:05
description: "This is a small guide to bascially secure a Linux server. I gather bits and pieces
around the web and list them here"
categories: linux
image: false
article: yes
tags: ['secure', 'linux', 'server']
---

* my toc
{:toc}


# Accounts

## Change Root Password


{% highlight bash linenos=table %}
ssh root@yourserveradress
passwd
{% endhighlight %}


## create a user account

{% highlight bash linenos=table %}
useradd -m work
mkdir /home/work/.ssh
chmod 700 /home/work/.ssh
{% endhighlight %}

## grant sudo rights to the user account



{% highlight bash linenos=table %}
visudo
{% endhighlight %}

Comment all existing user/group grant lines and add:

{% highlight bash linenos=table %}
root    ALL=(ALL) ALL
work  ALL=(ALL) ALL
{% endhighlight %}

Connect to your user account and test the sudo command

{% highlight bash linenos=table %}
su work
sudo apt-get update
#asks for your user password
{% endhighlight %}

Should work properly.


## Ssh connectivity

### On your local machine

If your keys are not created yet:

{% highlight bash linenos=table %}
ssh-keygen -t rsa
#enter a passphrase
{% endhighlight %}

>Using a key without a passphrase is basically the same as writing down that random
>password in a file on your computer. Anyone who gains access to your drive has gained
>access to every system you use that key with.

Your public key is now in ~/.ssh/id_rsa.pub

Add the contents of the id_rsa.pub on your local machine and any other public keys that
you want to have access to this server to this file.

{% highlight bash linenos=table %}
vim /home/work/.ssh/authorized_keys
chmod 400 /home/work/.ssh/authorized_keys
chown work:work /home/work -R
{% endhighlight %}

Test diconnect from your server and try to reconnect to the server with the nexly created
account.

{% highlight bash linenos=table %}
ssh work@yourserveradress
#ask for your passphrase
sudo apt-get update
#ask for your user password
{% endhighlight %}

If everything works properly there, it's time to disable root and password login.
Be sure to be connected with your user account (forget your root account).

## Prevent password and root login

{% highlight bash linenos=table %}
sudo vim /etc/ssh/sshd_config
{% endhighlight %}

Add these lines to the file
{% highlight bash linenos=table %}

PermitRootLogin no
PasswordAuthentication no

#If you connect to your server from a fix ip adress.
#Insert the ip address from where you will be connecting:

AllowUsers work@(your-ip) work@(another-ip-if-any)
{% endhighlight %}

Now restart ssh:

{% highlight bash linenos=table %}
sudo service ssh restart
{% endhighlight %}

Test it, from your local computer:

{% highlight bash linenos=table %}
$ ssh root@yourserveradress
Permission denied (publickey).
{% endhighlight %}



## Keeping your server fresh

{% highlight bash linenos=table %}
apt-get install unattended-upgrades
vim /etc/apt/apt.conf.d/10periodic

APT::Periodic::Update-Package-Lists "1";
APT::Periodic::Download-Upgradeable-Packages "0";
APT::Periodic::AutocleanInterval "7";
APT::Periodic::Unattended-Upgrade "1";

vim /etc/apt/apt.conf.d/50unattended-upgrades

// Automatically upgrade packages from these (origin:archive) pairs
Unattended-Upgrade::Allowed-Origins {
    "${distro_id}:${distro_codename}-security";
};
{% endhighlight %}

## TODO: Logwatch

{% highlight bash linenos=table %}
apt-get install logwatch
vim /etc/cron.daily/00logwatch
#add this line:
/usr/sbin/logwatch --output mail --mailto test@gmail.com --detail high
{% endhighlight %}

## IPTables

### cleanup

{% highlight bash linenos=table %}
# modifiy default policy to ACCEPT
sudo iptables -t filter -P INPUT ACCEPT
sudo iptables -t filter -P FORWARD ACCEPT
sudo iptables -t filter -P OUTPUT ACCEPT
# flush specific rules
# remember to do it after INPUT ACCEPT or your connection will be cut
sudo iptables -t filter -F
{% endhighlight %}

### White list

{% highlight bash linenos=table %}
# don't block established connections
iptables -A INPUT -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
# loopback
iptables -A INPUT -i lo -j ACCEPT
# ssh
iptables -A INPUT -p tcp -i eth0 --dport ssh -j ACCEPT
# jekyll
iptables -A INPUT -p tcp -m tcp --dport 4000 -j ACCEPT
# webserver
iptables -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
{% endhighlight %}

### Accept ping

{% highlight bash linenos=table %}
iptables -A OUTPUT -p icmp -m conntrack --ctstate NEW,ESTABLISHED,RELATED -j ACCEPT
iptables -A INPUT -p icmp -j ACCEPT
{% endhighlight %}


### Block everything else

{% highlight bash linenos=table %}
iptables -t filter -P INPUT DROP
iptables -t filter -P FORWARD DROP

{% endhighlight %}

### Show rules

{% highlight bash linenos=table %}
iptables -L -v -n
{% endhighlight %}

### Delete a rule

{% highlight bash linenos=table %}
iptables -L --line-numbers
#synthax: iptables -D <chain> <rule number>  ex:
iptables -D OUTPUT 2
{% endhighlight %}


### Reapply fail2ban rules

{% highlight bash linenos=table %}
sudo /etc/init.d/fail2ban restart
{% endhighlight %}

