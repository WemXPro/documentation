---
title: Licensing Service
description: This guide explains how the licensing service works
published: true
date: 2024-01-05T11:44:26.007Z
tags: 
editor: markdown
dateCreated: 2024-01-01T21:07:56.320Z
---

# Licensing Service

The licensing service is meant for developers that want to protect their software with a license system. This type of license system will only work for software that allows direct access to all the files related to that software, some examples are CMS's, SaaS projects, Discord Bots or any other project that can be downloaded.

With this license system, you can add specific checks inside your software by performing an API calls to the license server to validate license. If the license server returns a negative response, you can handle the response as needed.

# Public API

The public api can be used to perform checks that don't require any type of authentication i.e validate the license, or download a software.

## Validate a specific license

Endpoint
```
POST
/api/v1/licenses/public/validate
```

Data
```json
{
  "license": "LCE-e75803b9-1b3a-4801-a3ea-30cc1eb0e4b2",
  "domain": "demo.example.net", // the domain can be added on the license edit page
  "packages": "Example" // the name of the package for the license, separate multiple packages with a comma ","
}
```

Optional Data
- data (array): Pass additional data that's stored in the license
- transaction_id: Pass a unique transaction id, or external id, if this id already exists the api gives an invalid response
- due_date: pass a date value to set the due date after the license is created. The date should be in the future, and price should be a be of type "recurring"

Example Response
```json
{
    "success": true,
    "message": "License verified successfully.",
    "package": "Example",
    "type": "production",
    "license": {
        "id": 333,
        "order_id": 453,
        "key": "LCE-e75803b9-1b3a-4801-a3ea-30cc1eb0e4b2",
        "max_domains": 1,
        "last_installed_at": "2024-01-01T15:29:35.000000Z",
        "created_at": "2023-12-28T15:45:15.000000Z",
        "updated_at": "2024-01-01T15:29:35.000000Z"
    }
}
```

If its the first time the user performs an install on the given domain, the license server will automatically update the IP address. If a new request comes in the license server will compare the ip addresses to determine whether its the same machine or a new one. In the event that its a different machine, an error will be thrown.

## Retrieve all authorized domains 

Endpoint
```
GET
/api/v1/licenses/public/LCE-e75803b9-1b3a-4801-a3ea-30cc1eb0e4b2/domains
```

Example Response
```json
{
    "success": true,
    "domains": [
			{
        "id": 16,
        "license_id": 3,
        "type": "production",
        "protocol": "https:\/\/",
        "domain": "demo.example.net",
        "last_used_at": "2024-01-01T20:55:13.000000Z",
        "created_at": "2024-01-01T20:54:41.000000Z",
        "updated_at": "2024-01-01T20:55:13.000000Z"
    },
	{
        "id": 17,
        "license_id": 3,
        "type": "development",
        "protocol": "https:\/\/",
        "domain": "dev.example.net",
        "last_used_at": "2024-01-01T20:55:13.000000Z",
        "created_at": "2024-01-01T20:54:41.000000Z",
        "updated_at": "2024-01-01T20:55:13.000000Z"
    },
    ]
}
```

# Downloads (optional)

This license system also has a unique feature that allows you to return downloads over an API making them secure. You can create new downloads from the admin area -> licenses -> downloads

![screenshot_2024-01-01_230527.png](/assets/screenshot_2024-01-01_230527.png)

If you allow "Downloadable through web" users can download the files straight from the dashboard (see screenshot)

![screenshot_2024-01-01_230716.png](/assets/screenshot_2024-01-01_230716.png)

You can also enable the "Downloadable through API" method that is much more secure, there's an example API call below to do this:

Endpoint
```
POST
/api/v1/licenses/public/download
```

Data
```json
{
	"license": "LCE-e75803b9-1b3a-4801-a3ea-30cc1eb0e4b2",
  	"domain": "demo.wemx.net",
  	"packages": "Example",
  	"resource_name": "example"
}
```

Example Response
```json
{
    "success": true,
    "message": "Download started.",
    "download_url": "https:\/\/example.net\/api\/v1\/licenses\/public\/download\/NQQsdX8Tg97VOhMHMasedqniSKNuBt4xfDUOX0V0ef7mcWJ"
}
```

The license server first performs check to determine whether the license has access to the api url, after every check is successfull the license server generates a download url. You may use this url in automated scripts to download the zip file, extract it and delete it

# Private API

The private API is protected with bearer authentication and is only meant for admins to generate or extend licenses. You can generate an API key in the Admin area -> Licenses -> settings -> click "View API Key"

You are required to pass the bearer token with each api request to the private api, you can specify the token in the header in the example below:

Headers
```json
{
	"Authorization": "Bearer APIKEY" // replace APIKEY with your API Key
}
```

## Create a license

Endpoint
```
POST
/api/v1/licenses/private/create
```

Data
```json
{
	"email": "mfmjqpfbnaqnrpxrgx@cazlq.com",
  	"domain": "example.net",
  	"package_id": 84,
  	"price_id": 122
}
```

Example Response
```json
{
    "success": true
}
```

The license server first checks whether a user with the provided email already exists, if the user does not exists, it creates a brand new account for them and a random password and emails it to them, and creates a brand new license for that user.

- package_id: The package id is the ID of the package, the package must use the "license" service
- price_id: The price id is the id of the price, the price id must be from the package provided.

## Update License Due Date

Endpoint
```
POST
/api/v1/licenses/private/update-due-date
```

Data
```json
{
	"order_id": 232,
	"due_date": "2028-01-01T20:55:13.000000Z" // any date in the future
}
```

Example Response
```json
{
    "success": true
}
```

- order_id: ID of the license order
