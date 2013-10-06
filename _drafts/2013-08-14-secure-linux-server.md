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

## Modify sshd default port

###TODO

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
iptables -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT
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

###  persists rules

Your iptables configuration will be erased at each reboot if you don't persist it.

There are [many ways to do it][iptables_persist].

My favorite:

{% highlight bash linenos=table %}
#save actual rules
sudo iptables-save > /etc/iptables.rules
#create a script to be launch on boot before activating networking
sudo vim /etc/network/if-pre-up.d/firewallup
{% endhighlight %}

Script content:

{% highlight bash linenos=table %}
#!/bin/sh
iptables-restore < /etc/iptables.rules
exit 0
{% endhighlight %}

Make your script executable and test it
{% highlight bash linenos=table %}
sudo chmod +x /etc/network/if-pre-up.d/firewallup
sudo sh /etc/network/if-pre-up.d/firewallup
#check your tables
sudo iptables -L -n -v
#if everything is ok, test it for real
sudo reboot
{% endhighlight %}

Done with the firewall

# Mysql


{% highlight bash linenos=table %}
vim /etc/mysql/my.cnf
#add this line in the [mysqld] section
[mysqld]
# disable network access
skip-networking
# disable access to local files
local-infile=0

{% endhighlight %}

reboot service to check for configuration error

{% highlight bash linenos=table %}
service mysql restart
#check for errors in the log
tail -f /var/log/mysql/error.log
{% endhighlight %}

## Change root password

{% highlight bash linenos=table %}
shell> mysqladmin -u username -p password newpass
{% endhighlight %}

## drop test database

{% highlight bash linenos=table %}
mysql> drop database test;
{% endhighlight %}

## drop default user with no login/password


{% highlight bash linenos=table %}
mysql> use mysql;
mysql> select * from user where user="";
mysql> delete from user where user="";
mysql> flush privileges;
{% endhighlight %}


## Rename root account

{% highlight bash linenos=table %}
mysql> use mysql;
mysql> update user set user="new_user" where user="root";
mysql> flush privileges;
{% endhighlight %}

## Delete history

{% highlight bash linenos=table %}
cat /dev/null > ~/.mysql_history
{% endhighlight %}

## Then create specific user and database for your apps

{% highlight bash linenos=table %}
$ mysqladmin create dbname -u root -p
mysql> CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'new_password';
mysql> GRANT ALL PRIVILEGES ON dbname TO 'dbname'@'localhost' WITH GRANT OPTION;
mysql> GRANT ALL ON dbname.* TO 'new_user'@'somehost';
mysql> flush privileges;
#check grants
mysql> SHOW GRANTS FOR 'owncloud'@'localhost';
{% endhighlight %}

# Definitively stop useless services

# TODO: https

sudo a2enmod ssl
sudo service apache2 restart
sudo openssl req -nodes -days 364 -newkey rsa:1024 -keyout /etc/apache2/yourserver.key -out /etc/apache2/yourserver.csr
sudo chmod o-rw /etc/apache2/yourserver.key

sudo a2enmod rewrite
sudo a2dismod rewrite

# Openvpn

Follow tutorial for vpn configuation.

## IPtables rules for openvpn

{% highlight bash linenos=table %}
# Change port
iptables -A INPUT -p udp -m udp --dport 4000 -j ACCEPT
# Enable nat
iptables -t nat -A POSTROUTING -s 10.8.0.0/24 -o eth0 -j MASQUERADE
# activate forwarding
echo "1" > /proc/sys/net/ipv4/ip_forward
# Allow traffic initiated from VPN to access eth0
iptables -I FORWARD -i tun0 -o eth0 -s 10.8.0.0/24  -m conntrack --ctstate NEW -j ACCEPT
# Allow established traffic to pass back and forth
iptables -I FORWARD -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
{% endhighlight %}

## Check that your dns traffic is going through the vpn

{% highlight bash linenos=table %}
sudo tcpdump -i tun0 dst port 53
{% endhighlight %}

if not it may flow through your box

{% highlight bash linenos=table %}
sudo tcpdump -i tun0 dst port 53
{% endhighlight %}

Check that the follwing options are enabled in openvpn server.conf

{% highlight bash linenos=table %}
push "redirect-gateway bypass-dhcp"
push "dhcp-option DNS 208.67.222.222"
{% endhighlight %}

## Make sure vpn restart on server reboot

{% highlight bash linenos=table %}
vim /etc/default/openvpn
#uncomment AUTOSTART where server is the name of your server configuration aka server.conf
AUTOSTART="server"
{% endhighlight %}

## A valid server.conf

{% highlight bash linenos=table %}
/etc/openvpn# cat server.conf
port 2000
proto udp
dev tun
ca /etc/openvpn/ca.crt
cert /etc/openvpn/server.crt
key /etc/openvpn/server.key  # This file should be kept secret
dh dh1024.pem
server 10.8.0.0 255.255.255.0
ifconfig-pool-persist ipp.txt
comp-lzo
push "redirect-gateway bypass-dhcp"
#opendns
push "dhcp-option DNS 208.67.220.220"
push "dhcp-option DNS 208.67.222.222"
keepalive 10 120
max-clients 3
persist-key
persist-tun
status /etc/openvpn/openvpn-status.log
log /var/log/openvpn.log
verb 4
{% endhighlight %}

## Client conf

On ubuntu, use NetworkManager

[TODO](http://www.alsacreations.com/tuto/lire/622-Securite-firewall-iptables.html)

[iptables_persist]: https://help.ubuntu.com/community/IptablesHowTo#Configuration_on_startup
