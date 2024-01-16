---
title: WISP Module
description: Start selling wisp servers with the WISP module
published: true
date: 2024-01-16T18:21:47.799Z
tags: 
editor: markdown
dateCreated: 2024-01-16T18:03:19.809Z
---

# WISP Panel
WISP is a game panel based on https://pterodactyl.io/

Download WISP Service: https://wemx.net/resources/show/17
# Installation

Open the Admin area -> click "Services" -> Find WISP on the marketplace and install it. You can also manually download the WISP folder from wemx.net/marketplace and upload it to /path/to/wemx/app/Services/

# Setup

After installing the WISP service, it's time to set it up.

1. Setup configuration

Open the admin area -> click "Services" on the sidebar -> click "Configuration" button for WISP

- Hostname: The direct url to your WISP panel. (Example: https://panel.example.com)
- API Key: The Application API Key from the WISP Admin area, make sure you select read and write for all permissions
- Client API Key: The Client API key of an administrator, this can be generated in https://your-wisp.app/account/security (This is not the same as Application API Key!)

![wisp-configuration.png](/assets/wisp-configuration.png)

After successfully filling out all the config values click on "Test connection". If the response is a success, you can continue making packages.

# Creating a package

> Make sure that the locations you have selected contain nodes that have available allocations! You can use the "global stock" to limit purchases on WemX based on your available allocations
{.is-info}

After WISP is setup correctly, you can start creating packages.

To create a package, head to the admin area -> click "Products and Services" -> click "Packages" -> Create

Select WISP as service and create the package

After the package has been created, you can start setting up the wisp service configuration for the package.

Some important values to note are:

- Nest ID: The numeric ID of the nest the egg is in
- Egg ID: The numeric ID of egg you wish to use such as Minecraft, ARK etc.
- Locations: You can choose the locations the user can select at checkout. The "description" of the location is used as the display text. So to change the locations display name, you can change the desc on WISP

![wisp-package-config.png](/assets/wisp-package-config.png)

Fill in all the values such as DB limit, backup size etc and save the package
