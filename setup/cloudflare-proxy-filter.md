---
title: Cloudflare Proxy Filter
description: Limit traffic coming from CloudFlare
published: true
date: 2023-10-02T11:14:51.311Z
tags: 
editor: markdown
dateCreated: 2023-10-01T16:26:08.672Z
---

# Cloudflare Traffic Filter
If you are running WemX behind CloudFlare, you might want to enable the Cloudflare Proxy Filter. The filter limits traffic ONLY coming from CloudFlare, any traffic coming from outside cloudflare will be blocked. This is particularly usefull to protect your website against attackers.

Another benefit is that the real IP of the user connecting is saved. You may have noticed that the collected IPS of users look similar, that is because its showing cloudflares public IP when a user is making a connecting. You can find CloudFlares public IPs here: https://www.cloudflare.com/ips/ Enabling the filter allows you to get the real ip of users

To enable CloudFlare proxy filter edit your .env file: `nano /var/www/wemx/.env`

Check if `LARAVEL_CLOUDFLARE_ENABLED` exists in the ENV file, if it does not already exists add it like so and set the value to true below `APP_URL`. 
```
LARAVEL_CLOUDFLARE_ENABLED=true
```

> If you are enabling CloudFlare filter for the first time, you may want to delete the current IPs stored for users if they are coming from CloudFlare. To check this, compare the ips to the public ips from the link above. 
{.is-warning}

Deleting currently stored IPs: go to /var/www/wemx
```shell
php artisan tinker

UserIp::truncate();

exit
```