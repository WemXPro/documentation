---
title: Webserver Configuration
description: 
published: true
date: 2024-07-06T10:43:53.790Z
tags: 
editor: markdown
dateCreated: 2023-07-08T13:58:20.839Z
---

# Webserver
Webservers are responsible for returning content whenever a user makes a request to your website. To use WemX you need to choose which webserver you are going to use. 

# Nginx {.tabset}
## Nginx without SSL

> We recommend using the without SSL Nginx configuration. You can put your website behind CloudFlare proxy and CloudFlare will provide you with a SSL certificate.
{.is-info}

Start by creating a new nignx config file in the folder `/etc/nginx/sites-available` called `wemx.conf` - Below is a guide on how to create the file using nano, but similarly you can create it using SFTP.

```shell
nano /etc/nginx/sites-available/wemx.conf
```
Paste in the code below and replace `<domain>` with your domain

```nginx
server {
    # Replace the example <domain> with your domain name or IP address
    listen 80;
    server_name <domain>;


		# Path to WemX installation folder
    root /var/www/wemx/public;
    index index.html index.htm index.php;
    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    access_log off;
    error_log  /var/log/nginx/wemx.app-error.log error;

    # allow larger file uploads and longer script runtimes
    client_max_body_size 100m;
    client_body_timeout 120s;

    sendfile off;

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param PHP_VALUE "upload_max_filesize = 100M \n post_max_size=100M";
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param HTTP_PROXY "";
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

### Enabling configuration
Let's enable the configuration we just made and restart nginx for it to take effect
```shell
# You do not need to symlink this file if you are using CentOS.
sudo ln -s /etc/nginx/sites-available/wemx.conf /etc/nginx/sites-enabled/wemx.conf

# You need to restart nginx regardless of OS.
sudo systemctl restart nginx
```

> If your domains keeps redirecting you or does not show the proper website, check your php version with command `php -v` and update value on line 30 from 8.1 to your php version and restart nginx
{.is-warning}

## Nginx with SSL
> To use the Nginx with SSL configuration, you will need to generate a new SSL certificate for your domain. You can use services such as CertBot
{.is-info}

Start by creating a new nignx config file in the folder `/etc/nginx/sites-available` called `wemx.conf` - Below is a guide on how to create the file using nano, but similarly you can create it using SFTP.

```shell
nano /etc/nginx/sites-available/wemx.conf
```
Paste in the code below and replace `<domain>` with your domain

```nginx
server {
    listen 80;
    server_name <domain>;
    server_tokens off;
    return 301 https://$server_name$request_uri;
}

server {
    listen 443 ssl http2;
    server_name <domain>;

    root /var/www/wemx/public;
    index index.php;

    access_log /var/log/nginx/wemx.app-access.log;
    error_log  /var/log/nginx/wemx.app-error.log error;

    # allow larger file uploads and longer script runtimes
    client_max_body_size 100m;
    client_body_timeout 120s;

    sendfile off;

    # SSL Configuration - Replace the example <domain> with your domain
    ssl_certificate /etc/letsencrypt/live/<domain>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain>/privkey.pem;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers "ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384";
    ssl_prefer_server_ciphers on;

    # See https://hstspreload.org/ before uncommenting the line below.
    # add_header Strict-Transport-Security "max-age=15768000; preload;";
    add_header X-Content-Type-Options nosniff;
    add_header X-XSS-Protection "1; mode=block";
    add_header X-Robots-Tag none;
    add_header Content-Security-Policy "frame-ancestors 'self'";
    add_header X-Frame-Options DENY;
    add_header Referrer-Policy same-origin;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_split_path_info ^(.+\.php)(/.+)$;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
        fastcgi_index index.php;
        include fastcgi_params;
        fastcgi_param PHP_VALUE "upload_max_filesize = 100M \n post_max_size=100M";
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        fastcgi_param HTTP_PROXY "";
        fastcgi_intercept_errors off;
        fastcgi_buffer_size 16k;
        fastcgi_buffers 4 16k;
        fastcgi_connect_timeout 300;
        fastcgi_send_timeout 300;
        fastcgi_read_timeout 300;
        include /etc/nginx/fastcgi_params;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

### Enabling configuration
Let's enable the configuration we just made and restart nginx for it to take effect
```shell
# You do not need to symlink this file if you are using CentOS.
sudo ln -s /etc/nginx/sites-available/wemx.conf /etc/nginx/sites-enabled/wemx.conf

# You need to restart nginx regardless of OS.
sudo systemctl restart nginx
```

> If your domains keeps redirecting you or does not show the proper website, check your php version with command `php -v` and update value on line 47 from 8.1 to your php version and restart nginx
{.is-warning}


## Apache without SSL

> We recommend using the without SSL Apache configuration. You can put your website behind CloudFlare proxy and CloudFlare will provide you with a SSL certificate.
{.is-info}

> When using Apache, make sure you have the libapache2-mod-php package installed or else PHP will not display on your webserver.
{.is-warning}

Start by creating a new nignx config file in the folder `/etc/apache2/sites-available` or — if on CentOS, `/etc/httpd/conf.d/` called `wemx.conf` - Below is a guide on how to create the file using nano, but similarly you can create it using SFTP. 

```shell
nano /etc/apache2/sites-available/wemx.conf
```

Paste in the code below and replace `<domain>` with your domain

```apache
<VirtualHost *:80>
  ServerName <domain>
  DocumentRoot "/var/www/wemx/public"
  
  AllowEncodedSlashes On
  
  php_value upload_max_filesize 100M
  php_value post_max_size 100M
  
  <Directory "/var/www/wemx/public">
    AllowOverride all
    Require all granted
  </Directory>
</VirtualHost>
```

### Enabling configuration
Once you've created the file above, simply run the commands below. If you are on CentOS you do not need to run the commands below! You only need to run `systemctl restart httpd`

```shell
# You do not need to run any of these commands on CentOS
sudo ln -s /etc/apache2/sites-available/wemx.conf /etc/apache2/sites-enabled/wemx.conf
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## Apache with SSL

> To use the Nginx with SSL configuration, you will need to generate a new SSL certificate for your domain. You can use services such as CertBot
{.is-info}

> When using Apache, make sure you have the libapache2-mod-php package installed or else PHP will not display on your webserver.
{.is-warning}

Start by creating a new nignx config file in the folder `/etc/apache2/sites-available` or — if on CentOS, `/etc/httpd/conf.d/` called `wemx.conf` - Below is a guide on how to create the file using nano, but similarly you can create it using SFTP. 

```shell
nano /etc/apache2/sites-available/wemx.conf
```

Paste in the code below and replace `<domain>` with your domain

```apache
<VirtualHost *:80>
  ServerName <domain>
  
  RewriteEngine On
  RewriteCond %{HTTPS} !=on
  RewriteRule ^/?(.*) https://%{SERVER_NAME}/$1 [R,L] 
</VirtualHost>

<VirtualHost *:443>
  ServerName <domain>
  DocumentRoot "/var/www/wemx/public"

  AllowEncodedSlashes On
  
  php_value upload_max_filesize 100M
  php_value post_max_size 100M

  <Directory "/var/www/wemx/public">
    Require all granted
    AllowOverride all
  </Directory>

  SSLEngine on
  SSLCertificateFile /etc/letsencrypt/live/<domain>/fullchain.pem
  SSLCertificateKeyFile /etc/letsencrypt/live/<domain>/privkey.pem
</VirtualHost> 
```

### Enabling configuration
Once you've created the file above, simply run the commands below. If you are on CentOS you do not need to run the commands below! You only need to run `systemctl restart httpd`

```shell
# You do not need to run any of these commands on CentOS
sudo ln -s /etc/apache2/sites-available/wemx.conf /etc/apache2/sites-enabled/wemx.conf
sudo a2enmod rewrite
sudo a2enmod ssl
sudo systemctl restart apache2
```
