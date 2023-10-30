---
title: Migrating to WemX
description: This guide goes over the migration process from other services such as Billing v1, Billing v2, WHMCS, Blesta etc.
published: true
date: 2023-10-30T14:32:03.619Z
tags: 
editor: markdown
dateCreated: 2023-07-08T20:53:04.565Z
---

# Migrating to WemX

This guide goes over the migration process to WemX from various services such as Billing v1, Billing v2, WHMCS, Blesta etc...

# Tabs {.tabset}
## Billing v1 and v2

> The new WemX Pro, unlike earlier Billing v1 or v2 versions, operates independently of Pterodactyl and has a unique codebase. Currently, only user, address, and balance migrations are supported.
> If you haven't already migrated users from the installation, you can do so with the command below.
> ```shell
> php artisan import:pterodactyl:users
> ```

### Migrating Active Clients

> Make sure you have manually created a new category, package and setup the Pterodactyl API details before you begin with the process below.
{.is-warning}


Users must have an account on your WemX application before you can transfer them.
Transferring active orders from Billing v1 and v2 is pretty straigt forward. 
1. Head over to `Admin area -> orders` and click `create`

2. Select the package and price for your customer, set a due date corressponding to their original due date. Do not check the "Create an instance of package service" checkbox if your client already has a server. Create the order -- Check the screenshot below
![create_order.png](/assets/migrating/create_order.png)

3. Copy the orders ID

4. Head over to Pterodactyl and find that users server, edit the server and go to the "details" tab. Change "External ID" to "wmx-x" replace "x" with the WemX order ID i.e "wmx-23" as shown in the screenshot below 

![edit-external-id.png](/assets/migrating/edit-external-id.png)

5. You're done -- Your order is now handled by WemX. If the order expires WemX will suspend the server. -- Optionally, if you want to test whether the connection is working you can press "suspend" on the order from the admin area and see if it gets suspended on Pterodactyl.