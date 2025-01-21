---
title: Pterodactyl
description: This page documents the process to setup Pterodactyl integration for WemX
published: true
date: 2025-01-21T17:41:50.945Z
tags: 
editor: markdown
dateCreated: 2023-07-09T12:56:38.061Z
---

# Setting up Pterodactyl

Pterodactyl® is a free, open-source game server management panel built with PHP, React, and Go. Designed with security in mind, Pterodactyl runs all game servers in isolated Docker containers while exposing a beautiful and intuitive UI to end users.

You can use the official built-in Pterodactyl plugin to manage orders. WemX allows customers to setup enviroment variables at checkout, select a server location and more. There is also a SSO feature allowing your users to directly login to Pterodactyl with a single redirect.

> If you update the service before this, be sure to enable templates by default{.is-danger}

## Configuration

1. Open your Pterodactyl Panel, login as administrator and click on Application API. Click "Create New".

2. Set Read and Write for all fields and for description set "wemx" or any description you want (view screenshot below), hit Create Credentials. 
API key
![pterodactyl-config.png](/assets/third-party/pterodactyl-config.png)
Admin private API key
![user-api.png](/third-party/user-api.png)

3. On your WemX application, head to the Admin Area select the Pterodactyl dropdown and head over to settings. Paste in your API key and set Pterodactyl URL to your panel url. Make sure the Panel url starts with https:// or http:// and does NOT end with a '/'. Correct format: https://panel.example.com

![config.png](/assets/third-party/config.png)

4. In order for the console and other pages to work for the client, you need to configure Wings.

> **You must do this step for all your wings instances**
{.is-warning}


```shell
nano /etc/pterodactyl/config.yml
```
> Edit the `allowed_origins` field and add your WemX panel domain.
> **Make sure you enter your actual domain where you have WemX installed**
![wings-config.png](/third-party/wings-config.png)
It is also necessary that port 8080 is open or another port depending on your settings

> Be sure to restart Wings after making changes to config.yml.{.is-danger}
```shell
service wings restart
# or
sudo systemctl restart wings
```

> Pterodactyl requires SSL to use the console api, so you also need to edit the WemX panel .env file `nano /var/www/wemx/.env` and add the `FORCE_HTTPS=true` option or edit if it exists.{.is-warning}

> If you have any problems with the automatic installation. Remove the service manually app/Services/Pterodactyl after that install the service and run the commands below{.is-danger}
```
php artisan optimize:clear
php artisan wemx:update
php artisan module:publish
php artisan optimize:clear
```

5. Pterodactyl is successfully setup, you can start making new packages. 
![console.png](/third-party/console.png)

## Pterodactyl SSO

No more messing around with passwords and emails. With our SSO package for Pterodactyl, you can allow clients to login to Pterodactyl with just a press of a button. 

Once you setup the SSO feature, on the order management page on your application, your clients will see a "Login to Panel" button which they can click to be directly redirected and logged to Pterodactyl.

https://github.com/WemxPro/sso-pterodactyl

### Installation

Login to your Pterodactyl machine, and head to `/var/www/pterodactyl`
Download the package using Composer
```shell
cd /var/www/pterodactyl

composer require wemx/sso-pterodactyl
```

Publish the configuration file by running the following command:
```shell
php artisan vendor:publish --tag=sso-wemx
```

Generate a new SSO secret key which will be stored in the .env file. Run the command again if you wish to regenerate the key
```shell
php artisan wemx:generate
```

Clear cache and set correct file permissions
```shell
php artisan cache:clear && php artisan config:clear

php artisan route:clear

chmod -R 755 storage/* bootstrap/cache/

chown -R www-data:www-data /var/www/pterodactyl/*
```

Head over to your Pterodactyl Configuration located in the Admin area of your application. Paste in your SSO key and save it. (view screenshot below)


> The Pterodactyl SSO composer package is uninstalled from your Pterodactyl Panel when you update Pterodactyl. You must reinstall it again using the composer command above and clear cache.
{.is-warning}







