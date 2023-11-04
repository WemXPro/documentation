---
title: Setting up Captcha
description: This guide goes over the process to setup Captcha on your WemX application.
published: true
date: 2023-11-04T17:42:21.723Z
tags: 
editor: markdown
dateCreated: 2023-07-08T21:45:15.032Z
---

# Captcha
CAPTCHA (Completely Automated Public Turing test to tell Computers and Humans Apart) is a type of challenge-response test used on the internet to determine whether a user is human or a bot. It serves to prevent spam and abuse by bots on websites and can take various forms, including distorted text, image recognition, simple math problems, or audio sequences.

# Tabs {.tabset}
## CloudFlare Turnstile

Turnstile delivers frustration-free, CAPTCHA-free web experiences to website visitors - with just a simple snippet of free code. Moreover, Turnstile stops abuse and confirms visitors are real without the data privacy concerns or awful UX that CAPTCHAs thrust on users.

![Turnstile](/assets/captcha/turnstile.gif)

1. Head over to https://dash.cloudflare.com/ on the sidebar select "CloudFlare Turnstile"

2. Generate new credentials for your domain

3. Head over to your WemX installation `Admin Area -> Configuration -> Captcha` and enter your CloudFlare Turnstile details. Enable the Captcha and select the pages you want it active on.

You are done!

If you locked yourself out of your application, for example if the CloudFlare config is invalid, you can run the commands below to disable the CAPTCHA

```
cd /var/www/wemx
php artisan tinker
Settings::forget('encrypted::captcha::cloudflare');
```