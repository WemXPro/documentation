---
title: Installation
description: This page goes in depth on the installation process
published: true
date: 2023-12-12T21:05:21.020Z
tags: 
editor: markdown
dateCreated: 2023-07-08T10:03:10.397Z
---

# Getting Started

WemX is built on top of the Laravel framework an can be installed almost anywhere. This page documents the installation process for Ubuntu. You are expected to carefully read our documentation to ensure everything is setup correctly. You are expected to have basic knowledge of Linux, if you do not consider using one of our hosted plans(soon).

> WemX provides hosted plans that are deployed instantly on our systems with full file and database access. This services is specifically meant for those that do not have in depth understanding of Linux.
{.is-info}

### YouTube Video 

# Dependencies

- PHP `8.1` or above with the following extensions: cli, openssl, gd, mysql, PDO, mbstring, tokenizer, bcmath, xml or dom, curl, zip
- MySQL `5.7.22` and higher (MySQL 8 recommended) or MariaDB `10.2` and higher.
- A webserver (Apache, NGINX, Caddy, etc.)
- `curl`
- `zip` and `unzip`
- `composer` v2

## Example dependencies installation

> **Pterodactyl**
> If you are installing WemX on the same machine where Pterodactyl is already running, it is likely that most of the required dependencies are already installed. Therefore, you can skip this step.
{.is-warning}

The instructions provided here are just a representative illustration of how you can set up these dependencies. It's crucial to refer to your operating system's package manager to identify the right packages that need to be installed.

```shell
# Add "add-apt-repository" command
apt -y install software-properties-common curl apt-transport-https ca-certificates gnupg

# Add additional repositories for PHP, and MariaDB
LC_ALL=C.UTF-8 add-apt-repository -y ppa:ondrej/php

# MariaDB repo setup script can be skipped on Ubuntu 22.04
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash

# Update repositories list
apt update

# Add universe repository if you are on Ubuntu 18.04
apt-add-repository universe

# Install Dependencies
apt -y install php8.1 php8.1-{common,cli,gd,mysql,mbstring,bcmath,xml,fpm,curl,zip} mariadb-server nginx tar unzip git
```

## Installing composer

Composer is a dependency manager for PHP that allows us to ship everything you'll need code wise to operate the Panel. You'll need composer installed before continuing in this process.

```shell
curl -sS https://getcomposer.org/installer | sudo php -- --install-dir=/usr/local/bin --filename=composer
```

# Installation

## Create new project
Head over to the folder where you want to install WemX inside of. In this case we want to install WemX in `/var/www/wemx` so head over to `/var/www` - In this folder we will be creating a brand new Laravel project.

```shell
# cd into the folder you want to install WemX in
cd /var/www

# Create a new project with the folder name wemx
composer create-project laravel/laravel wemx
cd wemx
```

## Download WemX using installer
At this point we have created a brand new Laravel project, now we will be installing WemX.
```shell
# Download the WemX Installer using composer
composer require wemx/installer dev-web

# Initiate the installer
php artisan wemx:install
```

## Configure environment
```shell
# Create a new environment file
cp .env.example .env

# Download neccessary dependencies using composer
composer install --optimize-autoloader
```

### Setup encryption key
> **Danger**
> Only generate a new encryption key if you are installing WemX for the first time
> Encryption key is used to encrypt data that is stored in your database. After generating it, store it somewhere safe. You can find it in `.env` file under `APP_KEY`
{.is-danger}

```shell
# Only run this command if you are installing WemX for the first time
# and do not have any data in the database
php artisan key:generate --force
```

### Setup environment variables
The installer has downloaded all the necessary files for WemX. Let's configure the environment.
Make sure you have created a new database for WemX

> **Database Configuration**
>
> You will need a database for WemX. 
> ```shell
> mysql -u root -p
> 
> # Remember to change 'yourPassword' below to be a unique password
> CREATE USER 'wemx'@'127.0.0.1' IDENTIFIED BY 'yourPassword';
> CREATE DATABASE wemx;
> GRANT ALL PRIVILEGES ON wemx.* TO 'wemx'@'127.0.0.1' WITH GRANT OPTION;
> exit
>```

``` shell
# Setup the environment & Database
php artisan setup:environment && php artisan setup:database

# Enable default Modules & Make a symbolic link
php artisan module:enable && php artisan storage:link
```

### Migrate the database
Once you have setup the necessary environment variables, its time to migrate our database. 

```shell
# Migrate the database
php artisan migrate --force

# Set License key
php artisan license:update
```

### File Permissions
The last step in the installation process is to set the correct permissions on the files so that the webserver can use them correctly.
```php
# If using NGINX or Apache (not on CentOS):
chown -R www-data:www-data /var/www/wemx/*

# If using NGINX on CentOS:
chown -R nginx:nginx /var/www/wemx/*

# If using Apache on CentOS
chown -R apache:apache /var/www/wemx/*
```

### Setup Cron
Cron helps automatically perform certain tasks such as suspend/terminate orders, send renewal emails and much more. 

Run `sudo crontab -e` select nano and paste in the code at the bottom of the file.
```shell
* * * * * php /var/www/wemx/artisan schedule:run >> /dev/null 2>&1
```

### Create User
WemX is now completely installed. Let's get started by creating the very first user.
The very first user created on the application will be automatically made Administrator. You can later configure permissions for other admins users.

> **Pterodactyl**
>
> If you are migrating over from the previous Billing Module you can choose to import the users from the old billing module and your pterodactyl users.
> The following will be imported: Users, Addresses, Account Balance.
> 
>
> ```shell
> php artisan import:pterodactyl:users
> ```
> You can skip the step below.
<!-- {blockquote:.is-info} -->

```shell
php artisan user:create
```

# 🎉 You have successfully installed WemX

You have successfully installed WemX. Now all that's left to do is setup your [webserver configuration](https://docs.wemx.net/en/webserver)
