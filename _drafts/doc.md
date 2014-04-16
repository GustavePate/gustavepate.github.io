# Install

    sudo add-apt-repository ppa:nginx/stable
    sudo apt-get update
    sudo apt-get install nginx
    sudo service apache2 stop
    sudo apt-get remove apache2*
    cd /etc/nginx
    vim

Test conf:

    sudo nginx -t

# Todos

## forbid strange things

    server {
        listen      80;
        server_name "";
        return      444;
    }

## bot forbid sur ip

    server {
        server_name ip.ad.re.ess;
        return      444;
    }



## Error on non sub domain access

    server {
        server_name domain_name.tld;
        return      444;
    }


# Php 5 nginx

Install php

    apt-get install php5 php5-fpm

Add a server entry

    server{
        listen 80;
        server_name lych.localhost;

        # Make site accessible from http://localhost/

        index index.html index.htm index.php;
        root /var/www/php;

        location / {
            return 404;
        }

        location ~ [^/]\.php(/|$) {
            fastcgi_split_path_info ^(.+?\.php)(/.*)$;
            if (!-f $document_root$fastcgi_script_name) {
                return 404;
            }

            fastcgi_pass unix:/var/run/php5-fpm.sock;
            fastcgi_index index.php;
            include fastcgi_params;
        }
    }


## Nginx + lychee
## Nginx + transmission
## Nginx + Gateone

### Install gateone

mkdir ~/git/
cd ~/git

git clone https://github.com/liftoff/GateOne
sudo apt-get install python-pip debhelper python-support apt-file

sudo apt-file update
sudo python setup.py --command-packages=stdeb.command bdist_deb
cd deb_dist
sudo dpkg -i gateone*.deb
sudo gateone

sudo pip install tornado stdeb
sudo pip install --upgrade html5lib


cd /etc/gateone/conf.d/
vim 10server.conf


"origins": ["sub.yourdomain.com", "localhost", "127.0.0.1"],
"disable_ssl": true,
"url_prefix": "/somethinghardtoguess",



### Nginx conf

    #gateone
    server{
        listen 80;
        server_name sub.yourdomain.com;

        # Make site accessible from http://localhost/

        location / {
            return 404;
        }

        location ^~ /somethinghardtoguess/ {
            proxy_pass_header Server;
            proxy_set_header Host $http_host;
            proxy_redirect off;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Scheme $scheme;
            proxy_pass http://localhost:444/blabla/;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
            #proxy_cookie_path / /blabla/;
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            try_files $uri $uri/ =404;
            # Uncomment to enable naxsi on this location
            # include /etc/nginx/naxsi.rules
        }
    }


## Nginx + owncloud
# https everywhere

un seul server en 443
tous doivent avoir les ssl_certificates

# Logwatch nginx

    sudo vim /usr/share/logwatch/default.conf/logfiles/nginx.conf

    ########################################################
    # Define log file group for nginx
    ########################################################

    # What actual file? Defaults to LogPath if not absolute path….
    LogFile = nginx/*access.log
    LogFile = nginx/*access.log.1
    LogFile = nginx/*error.log
    LogFile = nginx/*error.log.1

    # If the archives are searched, here is one or more line
    # (optionally containing wildcards) that tell where they are…
    #If you use a “-” in naming add that as well -mgt
    Archive = nginx/*access.log*
    Archive = nginx/*error.log*

    # Expand the repeats (actually just removes them now)
    *ExpandRepeats

    # Keep only the lines in the proper date range…
    *ApplyhttpDate

    # vi: shiftwidth=3 tabstop=3 et

sudo cp /usr/share/logwatch/default.conf/services/http.conf /usr/share/logwatch/default.conf/services/nginx.conf


sudo mv /usr/share/logwatch/default.conf/services/http.conf /usr/share/logwatch/default.conf/services/http.conf.bak

sudo vim /usr/share/logwatch/default.conf/services/nginx.conf

Title = “nginx”
LogFile = nginx

sudo cp /usr/share/logwatch/scripts/services/http /usr/share/logwatch/scripts/services/nginx

logwatch restart


## Nginx + pyramid

ok

#secure nginx
/etc/nginx/nginx.conf
server_tokens off;

/etc/php5/fpm/php.ini:
expose_php = Off

curl -I your_url_here


mimic the $requets variable:

replace

    log_format combined '$remote_addr - $remote_user [$time_local] '
                       '"$request" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent";

by

    log_format compression '$remote_addr - $remote_user [$time_local] '
                       '"$request_method $request_uri $server_protocol" $status $bytes_sent '
                       '"$http_referer" "$http_user_agent" $https $request_time';



http://nginx.org/en/docs/http/ngx_http_core_module.html#variables
http://nginx.org/en/docs/http/ngx_http_log_module.html#log_format

