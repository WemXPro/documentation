---
title: Hestia
description: This guide goes through the process of installing Hestia service on WemX
published: true
date: 2023-11-02T19:58:36.723Z
tags: 
editor: markdown
dateCreated: 2023-11-02T19:58:36.723Z
---

# Hestia
Full hestia integration with WemX

Hestia Control Panel is designed to provide administrators an easy to use web and command line interface, enabling them to quickly deploy and manage web domains, mail accounts, DNS zones, and databases from one central dashboard without the hassle of manually deploying and configuring individual components or services. https://hestiacp.com/

Hestia Module: https://github.com/WemXPro/service-hestia

# Installing HestiaCP

> Hestia needs to be installed separately from WemX on a NEW VPS/Machine
{.is-warning}

To Install hestia on a new machine, follow this guide:
https://hestiacp.com/install.html

# Installing Hestia Service

1. Download the latest Hestia Service ZIP: https://github.com/WemXPro/service-hestia/archive/refs/heads/main.zip
2. Using FTP, locate folder /var/www/wemx/app/Services
3. Create a new folder called Hestia
4. Open the ZIP file downloaded earlier, then open `service-hestia-main` folder
5. Upload all files in this folder to the Hestia folder in `/var/www/wemx/app/Services/Hestia`
6. Update the Hestia dependencies with `php artisan module:update Hestia`

# Setup Hestia

After installing Hestia, header over to Admin -> Services -> Enable Hestia

![hestia-services.png](/assets/hestia-services.png)

After enabling Hestia, click on Configuration

![hestia-configuration.png](/assets/hestia-configuration.png)

- Hostname is the URL to your Hestia panel without port for example: http:// or https://hestia.example.com don't add a / at the end
- 
- Port is the port for your Hestia Panel, by default Hestia uses 8083, if you don't have any port set it to 443
- 
- Username is the Administrators username on the Hestia panel
- 
- Username is the Administrators password on the Hestia panel

Click on Update, then click on Test Connection.

If the connection test is successfull, Hestia has been setup successfully. 

## Debugging connection errors
If Hestia returns an error 403, make sure the password and username are correct. 

If Hestia returns error 401, make sure to whitelist your IP address
- Login to Hestia, click on ⚙️ top right
- Click on "⚙️ configure" button
- Select Secuirty -> System -> API
- Ensure API access is enabled for admins and Legacy API is enabled
- Add your WemX IP address in the Allowed IPs
- Save the settings, and test the API

If the error continues, check if the IP is not banned
- Login to Hestia, click on ⚙️ top right
- Click on "Firewall" button
- Click on "Banned IP Addresses"
- Check if your WemX application IP is in the list of banned IPs
