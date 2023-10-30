---
title: Creating Modules
description: This documentation describes the process of making Modules for WemX
published: true
date: 2023-10-27T19:05:54.710Z
tags: 
editor: markdown
dateCreated: 2023-02-16T07:45:54.688Z
---

# Modules
> This documentation is still Work in Progress and thus incomplete. For more accurate examples, please look at already published modules.
{.is-danger}

## Introduction

WemX comes with an advanced modular system. Modules allow you to extend capabilities of your WemX Panel, modules are essentially a mini Laravel applications with routes, providers, models, controllers, views and more. External developers can create Modules which can be downloaded by the community on our official marketplace: https://market.wemx.net

## Creating a new Module

1. Using SSH you may run the following command to generate a Module: 
`php artisan module:make <module-name>`

2. You may duplicate an existing Module, follow this link to copy a default layout

You will gave a newly created folder within in Folder `Modules/<module-name>`

## Building Modules

Once your module is created, you will find it in `Modules/<module-name>`. One of the most important files within your module folder is `Config/config.php`

This file will allow you to set the basics for your Module. You may configure the name, 

Example config:
```
<?php

return [

    'name' => 'Example Module',
    'icon' => 'https://imgur.png',
    'author' => 'WemX',
    'version' => '1.0.0',
    'wemx_version' => '1.0.0',

    'elements' => [

        'main_menu' => 
        [
            [
                'name' => 'Example',
                'icon' => '<i class="fa-solid fa-ticket-simple"></i>',
                'href' => '#',
                'style' => '',
            ],
            // ... add more menu items
        ],

        'user_dropdown' => 
        [
            [
                'name' => 'Example Module',
                'icon' => '<i class="fas fa-cog"></i>',
                'href' => '#',
                'style' => '',
            ],
            // ... add more menu items
        ],

        'admin_menu' => 
        [
            [
                'name' => 'Example Module',
                'icon' => '<i class="fas fa-cog"></i>',
                'href' => '#',
                'style' => '',
            ],
            // ... add more menu items
        ],

    ],

];
```

Elements allow you to dynamically add nav items & elements to the dashboard. 
You can add navigation menu directly through the config as well as modify them. You may add as many navigation routes as you wish.
