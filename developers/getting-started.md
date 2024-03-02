---
title: Getting Started Developers
description: This guide goes in depth about best practices for new developers getting started with WemX
published: true
date: 2024-03-02T20:50:28.056Z
tags: 
editor: markdown
dateCreated: 2024-03-02T20:50:28.056Z
---

# Getting Started (Developers Edition)

WemX is built by developers, for developers. While designing WemX, we tried our best to make it, as stream-lined and developer-friendly as possible for new developers getting started. In this short handguide, we will be laying out the fundamentals of WemX as well as provide you more information about its properties.

## Frontend

The term "frontend" or sometimes referred to as "client-side" is the interface users interact with while browsing through your web application. This includes everything the user can see on their screen such as buttons, cards, images. JavaScript is also a frontend language in most cases.

Frontend languages are rendered when a user visits your website. Users can see all the code for your frontend language through tools such as element inspect. This includes HTML, JavaScript and CSS. Therefore its important that you don't store or handle any sensitive information in client-side languages as it can be easily intercepted by malicious actors.

### Frontend Framework

WemX uses the following frontend frameworks:
- [Flowbite](https://flowbite.com/)
- [Tailwind](https://tailwindcss.com/)

### Templating Engine

Templating engine simplify the process of building themes and views. They allow you to more efficiently develop themes, make them organized and have many other benefits. WemX uses the Laravel Blade templating engine: https://laravel.com/docs/10.x/blade

### Icons

The default WemX theme uses https://boxicons.com/ (Font) and Flowbite SVGs https://flowbite.com/icons/

The admin theme uses https://fontawesome.com/v5/search (v5.15.4) for icons