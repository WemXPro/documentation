---
title: Queue Worker
description: This page goes in depth about the queue worker
published: true
date: 2024-02-01T16:48:44.413Z
tags: 
editor: markdown
dateCreated: 2024-02-01T16:48:44.413Z
---

# Queue Worker

The queue worker handles heavy tasks in the background to ensure your clients don't face slow loading pages or interruptions. This includes tasks such as sending webhooks, processing data and sending emails.

# Cron Queue (Default)

WemX by default has a queue worker that is started using Laravels Schedule system. This requires the cronjobs to be running.

## Enabling Cron Queue

Edit file `/var/www/wemx/.env`, update or add `CRON_QUEUE` and set it to `true`

```
CRON_QUEUE=true
```

If CRON_QUEUE doesn't exists in your .env file, then add it.

# Ubuntu Service Queue

You can also use Ubuntu's systemctl to run a service for the queue system. This option is more reliable but needs some additional setup. You may use this if the cron queue isn't working as intended.

Edit file `/var/www/wemx/.env`, update or add `CRON_QUEUE` and set it to `false`

```
CRON_QUEUE=false
```

Let's create the new queue worker.

```
nano /etc/systemd/system/wemx.service
```

Paste in the following: (Important: Replace /var/www/wemx/ with your wemx installation path if its different from default)

```
# WemX Queue Worker File
# ----------------------------------

[Unit]
Description=WemX Queue Worker

[Service]
# On some systems the user and group might be different.
# Some systems use `apache` or `nginx` as the user and group.
User=root
Group=www-data
Restart=always
ExecStart=/usr/bin/php /var/www/wemx/artisan queue:work --sleep=3 --tries=3
StartLimitInterval=180
StartLimitBurst=30
RestartSec=5s

[Install]
WantedBy=multi-user.target
```

Enable the service

```
sudo systemctl enable --now wemx.service
```

Finally, check the status of the queue worker using, check the response and make sure its running as expected.

```
systemctl status wemx.service
```

# Alternative Queue workers

WemX is built using Laravel meaning you can use a lot of other queue workers supported by Laravel. You can read more about the supported queue drivers here https://laravel.com/docs/10.x/queues