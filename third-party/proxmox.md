---
title: Proxmox
description: This guide explains how you can setup the proxmox integration with WemX
published: true
date: 2023-12-03T17:08:54.232Z
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

# Setup Proxmox service

After installing Proxmox, head over to Admin -> Services -> Enable Proxmox

![hestia-services.png](/assets/hestia-services.png)

After enabling Proxmox, click on Configuration

![proxmox-config.png](/assets/proxmox-config.png)

- Hostname: Enter the URL to your Proxmox panel including http:// or https:// and port
- ![Proxmox-panellogin.png](/assets/Proxmox-panellogin.png)


You can create a new token on your Proxmox panel under Datacenter -> API Tokens -> Add

![Datacenter.png](/assets/Datacenter.png)

then follow these screen shots to make a Api token
![addingAPITOKEN.png](/assets/addingAPITOKEN.png)


Step.2 Add a random password for your Api token random.org/passwords/?num=1&len=24&format=plain&rnd=new

Step 3: Insert your random characters into the API TOKEN ID field. Make sure to deselect the box in the screenshot and then click save.

![APITOKEN.png](/assets/APITOKEN.png)

Step 4: Head over to your store and access the admin panel. Navigate to 'Services Configuration' within Proxmox, then follow the provided screenshots for guidance

![steup_store.png](/assets/steup_store.png)

![store-Setup2.png](/assets/store-Setup2.png)

Step 5: After entering all information correctly, click on 'Update.' Proceed to test the connection. Once confirmed, you should be ready to create packages for your store

Step 6: If you encounter any issues after following these steps, don't hesitate to reach out to the WemX support chat in discord . Be sure to include details about the problem you're facing along with a screenshot of the error for better assistance






