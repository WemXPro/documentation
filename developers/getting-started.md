---
title: Getting Started Developers
description: This guide goes in depth about best practices for new developers getting started with WemX
published: true
date: 2024-03-02T21:34:37.324Z
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
- [Bootstrap](https://getbootstrap.com/) (Only Admin Theme)

### Templating Engine

Templating engine simplify the process of building themes and views. They allow you to more efficiently develop themes, make them organized and have many other benefits. WemX uses the Laravel Blade templating engine: https://laravel.com/docs/10.x/blade

### Icons

The default WemX theme uses https://boxicons.com/ (Font) and Flowbite SVGs https://flowbite.com/icons/

The admin theme uses https://fontawesome.com/v5/search (v5.15.4) for icons

## Backend

The term "backend" or sometimes referred to "server-side" is responsible for performing tasks that the user cannot see. Think of when a user submits a form, the backend validates the input and does something with that input, for example create an account.

### Backend Framework

You can think of a backend framework as a garage filled with all the possible tools you might need to build a project. Backend frameworks are similar in a way, they provide you with all the tools, a strong community and enhanced security.

WemX uses [Laravel](https://laravel.com/), an open-source framework built with php. WemX currently uses Laravel 10 and supports php 8.0 up to 8.2.

Laravel provides a lot of helpful classes to make development a lot easier
- Http
- Cache
- Collections
- Mail
- Str(ing)
- much more

These classes make development much easier and fun.

### Denotation

We encourage developers to follow the following denotation style.

#### Defining Variables

You should always give variables descriptive names and make sure they are unique and easy to remember. Don't start variables with a capital letter or numbers. Instead, make the first word lowered-cased and the second word capital.
```php
$first_name = 'John';

// or use camel case

$firstName = 'John';
```

If you want to merge variables with strings or other variables use double `""` rather than `''`
Example:

```php
$first_name = 'John';

$last_name = 'Smith';

$full_name = "{$first_name} {$last_name}"; // returns John Smith

// or

$full_name = $first_name .' '. $last_name; // returns John Smith
```

Merging two variables:

```php
$domain = 'https://example.com';
$endpoint = '/api/v1/users';

$url = $domain . $endpoint; // returns https://example.com/api/v1/users
```

Optionally, if you want force the output type of a specific variable, you may define it like so:

```php
$price = (float) 10; // returns 10.0
$age = (int) 24; // returns integer
$username = (string) 'johnsmith'; // returns string
$isAdmin = (bool) true; // returns boolean: true or false
$carBrands = (array) ['Mercedes', 'BMW', 'Audi']; // returns an array
$languages = (object) ['English', 'German', 'French']; // returns an object
```

#### Defining Functions

Functions are methods inside classes that handle specific tasks.

In Laravel, there are two types of functions

1. Static Functions
2. Non-static functions

Static functions can be called directly without having to create an instance of a class, while non static function require you to create an instance of the class before calling them.

We encourage you to start classes with a capital letter, and start functions with lower-cased letter and capitalize the second word similar to variables.

#### Static Functions

```php
<?php

namespace App\Facades;

use Illuminate\Support\Str;

class String
{
		public static function random(int $characters)
    {
    		return Str::random($characters);
    }
}
```
