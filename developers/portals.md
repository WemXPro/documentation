---
title: Portals
description: This documentation guide goes in detail on how portals function
published: true
date: 2023-12-17T16:36:53.132Z
tags: 
editor: markdown
dateCreated: 2023-12-17T16:36:53.132Z
---

# Portals

Portals function as the landing pages for your application. You can select one active portal page at once in the admin area under Admin -> configuration -> portals

WemX comes with a example portal by default that you can tailor to your needs. You can change the header image inside the portal settings in the admin area and also set the default category for which you wish to display prices for.

The files for portals are also separated from the theme files. Files are  located in resources/themes/portal/default for the default portal. 

Default Portal Files: https://github.com/WemXPro/portal-default

# Creating custom portal

To create your own portal

1. Navigate to /resources/themes/portal folder
2. Duplicate folder `default` into a new folder i.e `bootstrap`
3. Open the `bootstrap` folder, edit file portal.php and change the portal data such as the `folder` key to your folders name `bootstrap`
4. You can add your custom portal design in main.blade.php

# Using third party portals

You may also choose to use an external portal rather than the default one. This has many benefits such as that its easier to edit the portal design, add more pages. In this case, your WemX application would need to be installed on a different domain for example billing.example.com. Your portal lives on the root of the domain example.com, you can add a button to redirect users to the "billing area".

On WemX, open the portal settings in Admin -> configuration -> portals and set the portal to redirect, set the redirect url to /dashboard.

This will make it so the portal on WemX is no longer used, and redirects users to the dashboard, and guests to login or register.