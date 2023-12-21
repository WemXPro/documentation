---
title: Tickets Module
description: This guide documents the installation and setup for the tickets module
published: true
date: 2023-12-21T21:25:12.800Z
tags: 
editor: markdown
dateCreated: 2023-12-21T21:25:12.800Z
---

# Tickets Module
The Tickets module is a free module available for Platinum license holders. It allows your customers to submit support tickets on your application and has many features such as automatically responding to certain keywords, templates, email notifications, timeline, discord synchronization and much more.

# Installation

## Automatic Install

1. Open the admin area -> click on Modules -> click "Install"

![ticket-module-install.png](/ticket-module-install.png)

2. Confirm on the same page whether the "tickets" appears and click "Enable", in can take some seconds so keep refreshing

3. Migrate Database using: `php artisan migrate --force`

## Manual

1. Download the Tickets Module zip file from https://wemx.net/resources/show/6
2. Connect to your wemx application and open folder "Modules" from your WemX root directory
3. Open the zip file, upload "Tickets" folder into the "Modules" folder
4. Migrate Database using: `php artisan migrate --force`
5. Enable the Tickets module from the admin area on "Modules" tab

## Discord Sync