+++
author = "Nahid Hasan"
title = "Setup SSL for Nginx"
date = "2022-02-21"
description = "How to Setup SSL Certificate for Nginx"
tags = [
    "SSL", "Nginx",
]
+++


The system admins team of xFusionCorp Industries needs to deploy a new application on App Server 2 in Stratos Datacenter. They have some pre-requites to get ready that server for application deployment. Prepare the server as per requirements shared below:



Install and configure nginx on App Server 2.

On App Server 2 there is a self signed SSL certificate and key present at location /tmp/nautilus.crt and /tmp/nautilus.key. Move them to some appropriate location and deploy the same in Nginx.

Create an index.html file with content Welcome! under Nginx document root.

For final testing try to access the App Server 2 link (either hostname or IP) from jump host using curl command. For example curl -Ik https://<app-server-ip>/.



Solution:


# yum install   epel-release -y

# yum install   nginx -y

# vi /etc/nginx/nginx.conf

server {

        listen       80;

        listen       [::]:80;

        server_name  172.16.238.12;

        root         /usr/share/nginx/html;

 

        # Load configuration files for the default server block.

        include /etc/nginx/default.d/*.conf;

 

        error_page 404 /404.html;

        location = /404.html {

        }

 

        error_page 500 502 503 504 /50x.html;

        location = /50x.html {

        }

    }

 

# Settings for a TLS enabled server.

 

    server {

        listen       443 ssl http2;

        listen       [::]:443 ssl http2;

        server_name  172.16.238.12;

        root         /usr/share/nginx/html;

        ssl_certificate "/etc/pki/CA/certs/nautilus.crt";

        ssl_certificate_key "/etc/pki/CA/private/nautilus.key";

        ssl_session_cache shared:SSL:1m;

        ssl_session_timeout  10m;

        ssl_ciphers HIGH:!aNULL:!MD5;

        ssl_prefer_server_ciphers on;

 

#        # Load configuration files for the default server block.

        include /etc/nginx/default.d/*.conf;

 

        error_page 404 /404.html;

            location = /40x.html {

        }

 

        error_page 500 502 503 504 /50x.html;

            location = /50x.html {

        }

    }

 

}

# cp /tmp/nautilus.crt /etc/pki/CA/certs/

# cp /tmp/nautilus.key /etc/pki/CA/private/

# ll /usr/share/nginx/html/

# rm /usr/share/nginx/html/index.html

rm: remove symbolic link ‘/usr/share/nginx/html/index.html’? y

# vi /usr/share/nginx/html/index.html
[root@stapp03 ~]# ll /usr/share/nginx/html/

total 16

-rw-r--r-- 1 root root 3650 Jun  2 00:21 404.html

-rw-r--r-- 1 root root 3693 Jun  2 00:21 50x.html

lrwxrwxrwx 1 root root   20 Jul 25 11:56 en-US -> ../../doc/HTML/en-US

drwxr-xr-x 2 root root 4096 Jul 25 11:56 icons

lrwxrwxrwx 1 root root   18 Jul 25 11:56 img -> ../../doc/HTML/img

lrwxrwxrwx 1 root root   25 Jul 25 11:56 index.html -> ../../doc/HTML/index.html

-rw-r--r-- 1 root root  368 Jun  2 00:21 nginx-logo.png

lrwxrwxrwx 1 root root   14 Jul 25 11:56 poweredby.png -> nginx-logo.png

#

# rm /usr/share/nginx/html/index.html

rm: remove symbolic link ‘/usr/share/nginx/html/index.html’? y

#

# vi /usr/share/nginx/html/index.html

# cat /usr/share/nginx/html/index.html

Welcome!

# systemctl start nginx
# systemctl status nginx

$ curl -Ik https://stapp02

/tmp/nautilus.crt

/tmp/nautilus.key

