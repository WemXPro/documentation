---
title: Proxmox
description: This guide explains how you can setup the proxmox integration with WemX
published: true
date: 2023-12-03T17:08:54.232Z
tags: 
editor: markdown
dateCreated: 2023-12-03T17:08:09.490Z
---

# Installing the Proxmox Service

1. Download the latest Proxmox Service ZIP: https://github.com/WemXPro/service-proxmox/archive/refs/heads/main.zip
2. Using FTP, locate folder /var/www/wemx/app/Services and create a new folder called Proxmox
3. Open the ZIP file downloaded earlier, then open service-proxmox-main folder
4. Upload all files in this folder to the Proxmox folder in /var/www/wemx/app/Services/Proxmox
5. Update the Proxmox dependencies with php artisan module:update Proxmox

# Setup Proxmox service

After installing Proxmox, head over to Admin -> Services -> Enable Proxmox

![hestia-services.png](/assets/hestia-services.png)

After enabling Proxmox, click on Configuration

![proxmox-config.png](/assets/proxmox-config.png)

- Hostname: Enter the URL to your Proxmox panel including http:// or https:// and port

You can create a new token on your Proxmox panel under Datacenter -> API Tokens -> Add

https://cdn.discordapp.com/attachments/1074063210543579226/1181310126318170122/step_1_login_.png?ex=65930cc9&is=658097c9&hm=0206671118ac8e7a0c537ca0f6a052cce1ae0c4529e701d97e840c13e5ff6434&
Then follow these screen shots to make a Api token 
https://cdn.discordapp.com/attachments/1074063210543579226/1181310264977662014/part_5.png?ex=65930cea&is=658097ea&hm=455f6ef94c163fe74110c5eb0a7a8f52bc75f40ec075acc6cf934888eb809072&

https://cdn.discordapp.com/attachments/1074063210543579226/1181310294769811537/part6.png?ex=65930cf1&is=658097f1&hm=27f935cdf8b509a650e763751660f5ac59563b7a0051b6ce34997ed62fa5a229&

https://cdn.discordapp.com/attachments/1074063210543579226/1181310408590639225/SVAE_7.png?ex=65930d0d&is=6580980d&hm=cc34aeeed340bd4ab4161ab400c3e7e50f9325195e17e4799fee49538468df54&
u can use this link to genarate a token https://www.random.org/passwords/?num=1&len=24&format=plain&rnd=new
https://cdn.discordapp.com/attachments/1074063210543579226/1181310437753634886/TOKEN_6.png?ex=65930d14&is=65809814&hm=eac86efa2dc774dd117e8a7b57e8cd91d50ad4b548e4a91346bea46b35f5c49b&

Then login to your wemex store 
https://cdn.discordapp.com/attachments/1074063210543579226/1181310509031637092/steup_store.png?ex=65930d25&is=65809825&hm=2dd0238f1b04c809763846b971e28f233b5070ecf4c6d7655f916338d3e2f47d&

https://cdn.discordapp.com/attachments/1074063210543579226/1181310509434294395/store_2.png?ex=65930d25&is=65809825&hm=9667407dd92d8ae9725813bbe2bfdb3d3bdfc4e8cec0490073efc46bf01a42ef&

You can create a new token, and copy the token id and password once it has been generated.
