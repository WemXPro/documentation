---
title: Updating WemX
description: This guide goes in depth on how to update WemX
published: true
date: 2023-10-15T19:19:37.597Z
tags: 
editor: markdown
dateCreated: 2023-07-08T14:25:19.188Z
---

# How to Update

WemX comes with an installer which can be both used for installations and to receive updates. The installer directly connects to WemX API to receive the latest changes and any dependencies that are needed.

Note: Any changes made to core files including the default theme will get replaced by updates. It is recommended that you [create your own theme](/developers/themes) if you wish to make changes.

You should make a backup of the database and files before updating.
## Run Installer
```shell
# ensure you are in the wemx dir
cd /var/www/wemx

# Download latest available version of the installer
composer require wemx/installer dev-wemxpro

# Initiate updater
php artisan wemx:update
```

That's it you have successfully updated WemX to the latest available stable version.
{.is-info}
