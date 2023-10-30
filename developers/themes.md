---
title: Creating a Client Theme
description: This is a tutorial on how to create a Client Theme
published: true
date: 2023-10-27T19:05:33.587Z
tags: 
editor: markdown
dateCreated: 2023-02-16T04:24:41.236Z
---

> This documentation is still Work in Progress and thus incomplete. For more accurate examples, please look at already published themes.
{.is-danger}

# Introduction

Themes allow you to dynamically change the appearance of your panel without affecting any backend programs. 

# Creating a Client Theme
In this example, we will be creating a new client theme called Material 

1. Duplicate the default theme folder `resources/themes/client/default` to `resources/themes/client/material`
2. Inside the root of the newly created theme folder you will find a file called `theme.php` here you can specify the new name. Make sure to change the name to Material
3. Duplicate the default Affiliates folder `Modules/Affiliates/Resources/views/client/tailwind` to `Modules/Affiliate/Resources/views/client/material`
4. Duplicate the default Service folder `app/Services/Universal/Resources/views/client/tailwind` to `app/Services/Universal/Resources/views/client/material`
5. Activate the new theme in the Admin Area
6. You may customize the theme to your preference

## Facades Methods
```php
<?php
use App\Facades\Theme;

# You may use this function to return active theme view from Controller
Theme::view($path);

# You may use this function to return details about current active theme
Theme::active();

# You may use this function to retrieve information about a specific theme, if theme does not exists it will display the default theme
Theme::get($theme);

# You may use this function to retrieve view path to a specific theme, additionally you may pass optional variable $path to direct to a specific folder
Theme::getViewPath($theme, $path = NULL);

# You may use this function to retrieve all available themes
Theme::list();
```

# Creating an Admin Theme
In this example, we will be creating a new admin theme called Material 

1. Duplicate the default theme folder `resources/themes/admin/default` to `resources/themes/admin/material`
2. Inside the root of the newly created theme folder you will find a file called `theme.php` here you can specify the new name. Make sure to change the name to Material
3. Activate the new theme in the Admin Area
4. You may customize the theme to your preference

## Facades Methods
```php
<?php
# NOTE: In controllers that require both Theme facades, import AdminTheme without as Theme
use App\Facades\AdminTheme as Theme;

# You may use this function to return active theme view from Controller
Theme::view($path);

# You may use this function to return details about current active theme
Theme::active();

# You may use this function to retrieve information about a specific theme, if theme does not exists it will display the default theme
Theme::get($theme);

# You may use this function to retrieve view path to a specific theme, additionally you may pass optional variable $path to direct to a specific folder
Theme::getViewPath($theme, $path = NULL);

# You may use this function to retrieve all available themes
Theme::list();
```
