---
title: OAuth Configuration
description: This page documents configuration for OAuth
published: true
date: 2023-07-20T17:48:04.905Z
tags: 
editor: markdown
dateCreated: 2023-07-08T22:06:16.658Z
---

# OAuth Configuration

OAuth (Open Authorization) is a protocol that lets users authorize one website (the consumer, like Spotify) to access their data on another website (the provider, like Facebook), without sharing their login credentials. It uses secure tokens instead of passwords to grant access, thereby ensuring users' privacy and security.

On WemX users can connect their accounts to various services to display on their profile or to get certain permissions on servers such as Discord.

> At this moment, users can only link Discord, Github or Google to their account to be displayed on their profile. We will be introducing SSO logins shortly.
{.is-warning}


# Tabs {.tabset}
## Discord

1. Go to Discord [Developer Portal](https://discord.com/developers/applications) 

2. Click on "New Application" and enter an Application Name and click save

3. Now open the newly made application, and go to the OAuth2 section like in the screenshot below

![Discord](/assets/oauth/discord/index.png)

4. Add a new Redirect URL and enter the following url to redirect to `https://example.com/oauth/discord/redirect` replace with your domain

5. Click Save and Copy your Client ID, to see your Client Secret, you must click on "Reset Secret"

6. Go to `Admin Area -> Configuration -> OAuth` and paste in your details and enable Discord like in the screenshot below.

![WemX](/assets/oauth/discord/wemx.png)

Save the file, and you're done.

## GitHub

1. [Create OAuth Application](https://github.com/settings/developers) | Create and Retrieve Github OAuth credentials
2. Enter `https://example.com/oauth/github/redirect`  as your redirect - **Make sure to replace with your own domain**

![GitHub](/assets/oauth/github/index.png)

3. Go to `Admin Area -> Configuration -> OAuth` and paste in your details and enable Github like in the screenshot below.

![github.png](/assets/oauth/github/wemx.png)

Save the file, and you're done.

## Google
1. [Google Developer Portal](https://console.developers.google.com/apis/credentials) | Create and Retrieve Google OAuth2 credentials
2. Enter `https://example.com/oauth/google/redirect`  as your redirect - **Make sure to replace with your own domain**

![image](/assets/oauth/google/index.png)

3. Go to `Admin Area -> Configuration -> OAuth` and paste in your details and enable Google like in the screenshot below.

![google.png](/assets/oauth/google/wemx.png)

Save the file, and you're done.
