---
title: Tickets Module
description: This guide documents the installation and setup for the tickets module
published: true
date: 2023-12-21T21:38:26.226Z
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

The Discord sync feature allows you to syncronize tickets between your web application and discord. This is mainly meant for admins to allow them easier access and notify for new tickets.

1. Invite the bot to your Discord Server https://discord.com/api/oauth2/authorize?client_id=1182377435111641099&permissions=8&scope=bot
2. Open the "Settings" tab for Tickets module in the Admin Area and enable "Enable Discord Sync"
3. Paste in your Discord Servers ID and the channel ID where the tickets should be created on discord (See Screenshot)

![tickets-module-discord-settings.png](/tickets-module-discord-settings.png)

4. Open your Discord server, find a private channel and run `/sync` enter in your domain and the api key from tickets module settings
5. Create a new ticket on the web to see if everything is working correctly. It can take a couple of seconds for the ticket to be created on Discord