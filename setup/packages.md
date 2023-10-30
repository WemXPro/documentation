---
title: Packages
description: Learn how to properly setup packages
published: true
date: 2023-10-30T15:25:35.139Z
tags: 
editor: markdown
dateCreated: 2023-09-30T17:14:56.199Z
---

# Packages

Packages allow you to create dynamic products for the services you are selling. Each package has its own category in which it is displayed and has its own "Server" or "Service". 

WemX comes with two services by default: Universal and Pterodactyl.

## Feature list

Each package has a list of features shown at checkout & pricing cards. You can use icons for the features from https://boxicons.com/ or select icons from a default list of icons. To add custom icons to the default icons, you can edit file `config/utils.php`

You can also use custom SVG icons. Open the icon editor, and paste your SVG code. Make sure to include attribute `fill="currentcolor"` to prevent color issues.

## Prices

You can give users different pricing cycle options to choose from at checkout. You can choose from daily, weekly, quarterly, yearly all the way to every 10 years price cycle "period".

The "price" field indicates the initial price paid for the first payment for an order. The "Renewal Price" refers to the price paid per period. If you set the period to Monthly, and the renewal price to $15 that would mean the user has to pay $15 for every month. 

You can set an one-time setup fee the user must pay for the first payment. The cancellation fee refers to the price the user must pay to cancel their order i.e if they have an contract or such.

The "data" field is used for more advanced use cases such as setting an anchor price for subscriptions. For example, if you are using the "Paddle" gateway and want to set a specific product from paddle, you can link it using the data field.

The data field can also be used to set custom badges for example if you want to set a badge "Popular", "Discounted", "15% OFF" etc. To do this, set the data field to `{"badge": "Popular"}`

## Package Emails

You can set custom emails to be triggered for specific events such as when an order is created, terminated, suspended, or cancelled. In addition, you can attach custom files to the emails. This can be particularly usefull if you are selling files/software of some sort and you can send an email on the creation event containing the necessary files.

## Package Webhooks

Package webhooks are a powerful feature allowing you to do countless things on specific events. For instance, together with the Universal service, you could create a webhook at on the creation event creates a VPS for the user and on the suspend event suspends the vps. Another use is to give a user a role on creation, and remove it on suspend on discord. You can also setup discord webhooks or custom API endpoints.

You can use data to provide a json object to include in the request. Headers can be used for authentication.

Let's create a cool webhook to demonstrate its purpose. In this demonstration, we are going to create a webhook that notifies on creation that a specific user has made a purcahse

On your discord server -> go to a channel -> edit -> integrations
![Webhook](/assets/webhook/index.png)

Copy the Webhook URL and create a new webhook

Set the method to POST, in URL enter your discord webhook URL inside data add:
```
{
    "content": "$order->user->username just purchased $order->name",
    "embeds": [
        {
            "title": "Order Created: $order->package->name",
            "description": "$order->user->email just purchased a new server!",
            "color": "7506394"
        }
    ]
}
```

![Create Webhook](/assets/webhook/create.png)
Leave headers empty.

Now when a user places an order, a noticiation will be sent to Discord:
![Webhook Received](/assets/webhook/received.png)

You can use custom variables in webhooks identified by the `$order` operator. The $order operator has access to the user, package, payments and more.

Available variables:
```php
$order->id
$order->name
$order->status
$order->due_date
$order->price['price']
$order->price['renewal_price']
$order->due_date
$order->created_at
// more from orders model

$order->user->username
$order->user->email
// more from the user model

$order->package->name
$order->package->status
// more from packages model
```
