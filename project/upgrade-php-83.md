---
title: Upgrading PHP
description: This guide explains how you can upgrade php to a later version for WemX
published: true
date: 2024-10-05T14:12:44.523Z
tags: 
editor: markdown
dateCreated: 2024-10-05T14:12:44.523Z
---

# Upgrading PHP from 8.1 or lower to 8.3

In this guide, we'll look at how we can upgrade our PHP version from 8.1 to 8.3

Check your current PHP Version using `php -v`
```shell
root@vmi685053:~# php -v
PHP 8.1.24 (cli) (built: Oct  6 2023 09:46:19) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.1.24, Copyright (c) Zend Technologies
    with Zend OPcache v8.1.24, Copyright (c), by Zend Technologies
```

On line 2 of the output, we can see that we are running PHP 8.1

1. Let's upgrade this to PHP 8.3

```shell
# Update repositories list
apt update

# Install Dependencies
apt -y install php8.3 php8.3-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git
```

Now if we run `php -v` we'll see that we have upgraded to php 8.3
```shell
root@vmi685053:~# php -v
PHP 8.3.12 (cli) (built: Sep 27 2024 03:52:40) (NTS)
Copyright (c) The PHP Group
Zend Engine v4.3.12, Copyright (c) Zend Technologies
    with Zend OPcache v8.3.12, Copyright (c), by Zend Technologies
```
If you still see php 8.1, try to reboot your machine and delete the previous php version in folder /etc/php

2. If you are using Nginx, update the PHP version in the config

```shell
nano /etc/nginx/sites-available/wemx.conf
```

Change
```
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
```
to
```        
			fastcgi_pass unix:/run/php/php8.3-fpm.sock;
```

Now restart Nginx
```shell
systemctl restart nginx
```