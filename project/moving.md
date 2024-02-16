---
title: Moving WemX
description: 
published: true
date: 2024-02-16T08:52:35.123Z
tags: 
editor: markdown
dateCreated: 2024-02-16T08:52:35.123Z
---

# Moving WemX to a different server

1. Backup your hidden `.env` file containing the decryption APP_KEY from `/var/www/wemx`

2. Export the database, in this case ours is named **wemx**

    ```mysql
    mysqldump -u root -p --opt wemx > /var/www/wemx/wemx.sql
    ```

    The *.sql* file would be saved in the `/var/www/wemx/` folder.

3. Follow the [WemX Installation Guide](/project/installation) to install WemX on your new server.

4. Transfer the `wemx.sql` file to your new server and import the database. Make sure you're in the folder containing your *.sql* dump when performing the commands.

    ```mysql
    mysql -u root -p wemx < wemx.sql
    ```

5. After this, transfer your old `.env` file to the `/var/www/wemx` location to complete the moving process.