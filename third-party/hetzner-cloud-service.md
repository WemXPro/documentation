---
title: Hetzner Cloud
description: This guide explains how you can setup the hetzner cloud integration with WemX
published: true
date: 2024-06-11T18:02:03.070Z
tags: 
editor: markdown
dateCreated: 2024-06-11T16:40:07.506Z
---

# Installing the Hetzner Cloud Service

## Automatic
1. Go into you admin dashboard on your WemX Application
2. Category Design & Compatibility
3. And there to Services
4. Search for Hetzner Cloud Intigration
5. Click Install

## Manual
1. Download the latest Hetzner Service ZIP: https://wemx.net/resources/show/48
2. Using FTP, locate folder /var/www/wemx/app/Services and create a new folder called Proxmox
3. Open the ZIP file downloaded earlier, then open service-proxmox-main folder
4. Upload all files in this folder to the Proxmox folder in /var/www/wemx/app/Services/Proxmox
5. Update the Proxmox dependencies with php artisan module:update Proxmox


# Configure Hetzner service
After installing Hetzner Cloud Service, head over to Admin -> Services -> Enable Hetzner Cloud

Now head over to the Hetzner Site

When you in the Cloud Console its good to create a new project.

![create_new_project](https://cdn.gamestacks.de/go/h4qjyV)

If the project is created I recommend muting the project so you don't allways get notice about server.

![mute_project](https://cdn.gamestacks.de/go/mQqWZd)

# Getting API token

In order to use Hetzner Cloud you need a API key.

1. Open a project on Hetzner Cloud where you want to create the servers in
2. Go to the Security Tab

![security_tab](https://cdn.gamestacks.de/go/Duia5M)

3. Now click on API tokens.

![api_tokens_tab](https://cdn.gamestacks.de/go/uaaeWm)

4. Here you need to click on `Generate API token`

![generate_api_token](https://cdn.gamestacks.de/go/uvWGaI)

5. Give it a Name and `Read & Write` Permissions
6. Create the Token & Copy it
7. Head over to your WemX Application and and configure the Hetzner Service

# Create Plan

When you created a Plan with the Hetzner Service you need to configure 2 things

- Server Type
- Allowed Locations

You can find the Server Type when you creating a server in the Hetzner Cloud.

- That is the server type:
![server_type](https://cdn.gamestacks.de/go/yYsC4H)

The name should be all in small letters for example `cx11`

But you need to look if that server type is available at every location you set.

For the Allowed Locations you can just select them from the dropdon menu.
