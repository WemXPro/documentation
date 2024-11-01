---
title: Proxmox
description: This guide explains how you can setup the proxmox integration with WemX
published: true
date: 2023-12-30T18:11:47.209Z
tags: 
editor: markdown
dateCreated: 2023-12-03T17:08:09.490Z
---

# Installing the Proxmox Service

1. Download the latest Proxmox Service ZIP: https://github.com/WemXPro/service-proxmox/archive/refs/heads/main.zip
2. Using FTP, locate folder /var/www/wemx/app/Services and create a new folder called Proxmox
3. Open the ZIP file downloaded earlier, then open service-proxmox-main folder
4. Upload all files in this folder to the Proxmox folder in /var/www/wemx/app/Services/Proxmox
5. Update the Proxmox dependencies with php artisan module:update Proxmox

# Configure Proxmox service

After installing Proxmox, head over to Admin -> Services -> Enable Proxmox

![proxmox_config.png](/assets/proxmox_config.png)

After enabling Proxmox, click on Configuration

![proxmox-config.png](/assets/proxmox-config.png)

- Hostname: Enter the URL to your Proxmox panel including http:// or https:// and port
- Token ID: Enter token ID from API token for Proxmox
- Token Secret: Enter token secret from API token for Proxmox

# Creating API token on Proxmox

In order to setup the Proxmox service correctly on WemX, you need to create an API token on your Proxmox panel. 

1. Open your Proxmox panel -> Datacenter -> API Tokens and click "Add" API Token 

![proxmox_apitoken.png](/assets/proxmox_apitoken.png)

![proxmox_create_api_token.png](/assets/proxmox_create_api_token.png)

2. Set "wemx" as token ID and make sure the "Privilege Separation" option is NOT selected

![proxmox_create_token_id.png](/assets/proxmox_create_token_id.png)

3. After you have created the token successfully, Proxmox shows the details for the API token. Make sure to copy them right away as they don't show up again.

![proxmox_token_secret.png](/assets/proxmox_token_secret.png)

4. Open WemX and locate the configuration for Proxmox, fill in the token ID and token secret and the Proxmox URL. Save the configuration and click on "Test connection" to verify everything is working.

![proxmox_wemx_config.png](/assets/proxmox_wemx_config.png)

Here you can find a guide on manually adding VMs to orders in your Proxmox service: [Proxmox Manual VM Guide](https://www.youtube.com/watch?v=tg0bXrOjCBg)
