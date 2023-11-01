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

1. Duplicate the default theme folder `resources/themes/client/tailwind` to `resources/themes/client/material`
3. Inside the root of the newly created theme folder you will find a file called `theme.php` here you can specify the new name. Make sure to change the name to Material
4. Duplicate the default Universal Service folder `app/Services/Universal/Resources/views/client/tailwind` to `app/Services/Universal/Resources/views/client/material`
5. Duplicate the default Pterodactyl Service folder `app/Services/Pterodactyl/Resources/views/client/tailwind` to `app/Services/Pterodactyl/Resources/views/client/material`
6. Duplicate the default Affiliates module folder `Modules/Affiliates/Resources/views/client/tailwind` to `Modules/Affiliate/Resources/views/client/material`
7. Duplicate the default Locales module interface folder `Modules/Locales/Resources/views/client/tailwind` to `Modules/Locales/Resources/views/client/material`
8. Activate the new theme in the Admin Area
9. You may customize the theme to your preference

## Editing Client Theme Background Colors
In this example, we will be changing the background color from gray-900 (#111827) to neutral-900 (#171717)

Note: In this example, the custom client theme is called Material

1. Open resources/themes/client/material/layouts/tailwind.blade.php in your favourite text/code editor
2. Look for line 10:
```
gray: {"50":"#F9FAFB","100":"#F3F4F6","200":"#E5E7EB","300":"#D1D5DB","400":"#9CA3AF","500":"#6B7280","600":"#4B5563","700":"#374151","800":"#1F2937","900":"#111827"},
```
3. Change the hex value in "900" from "#111827" to "#171717":
```
gray: {"50":"#F9FAFB","100":"#F3F4F6","200":"#E5E7EB","300":"#D1D5DB","400":"#9CA3AF","500":"#6B7280","600":"#4B5563","700":"#374151","800":"#1F2937","900":"#171717"},
```
4. Save the file, and reload your page.

You should see the background color change to "#171717" after the reload.


These steps are also true for changing the background colors of the cards, but instead of the value in "900", the value in "800" should be changed:
```
gray: {"50":"#F9FAFB","100":"#F3F4F6","200":"#E5E7EB","300":"#D1D5DB","400":"#9CA3AF","500":"#6B7280","600":"#4B5563","700":"#374151","800":"#262626","900":"#171717"},
```

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
