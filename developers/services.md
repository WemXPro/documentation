---
title: Service Development
description: This documentation page goes in depth in regarding development of service for WemX
published: true
date: 2023-11-30T21:33:21.199Z
tags: 
editor: markdown
dateCreated: 2023-10-23T23:56:59.457Z
---

# Service Development

WemX references server integrations with the term "Service". A Service is module that can be used to make custom integrations between WemX and the (digital) product that you want to sell. Each package has its own service. The Service is typically invoked when a new order is created, suspended, unsuspended, or terminated.

Let us give you an example to better visualize this idea.
If you want to sell Minecraft servers, you would need a service that, upon purchase, sends an API request to your panel to generate the server. This request would be sent using the Service. Upon suspension, the service will send a suspend request through an API and so forth for other events.

Services are stored inside folder `app/Services`

Under the hood, just like Modules, Services utalize Laravel Modules, a module management package for Laravel: https://docs.laravelmodules.com/v10/introduction

Paired with Laravel, you can achieve anything you want using a Service. In theory, you are able to create a whole application using a single Service.


## Creating a Service

You can find a bare-bone example of a service here: https://github.com/WemXPro/service-example

WemX comes with a convenient command to create a custom service

In your WemX folder, run:
```shell
# Replace example with your name, replace spaces with underscore i.e "Direct Admin" is "direct_admin"
php artisan service:make Example
```
This command generates a new service with your name in `app/Services/<service-name>`

Each Service has a file called Service.php in its root directory. Here you can find an example of this file: https://github.com/WemXPro/service-example/blob/main/Service.php 

The Service.php file is the most important file that handles all the primary functions.

## Meta Data

Inside Service.php you have the following method:
```php
    /**
     * Returns the meta data about this Server/Service
     *
     * @return object
     */
    public static function metaData(): object
    {
        return (object)
        [
          'display_name' => 'Example',
          'author' => 'WemX',
          'version' => '1.0.0',
          'wemx_version' => ['dev', '>=1.8.0'],
        ];
    }
```

The meta data references information regarding the service. Such as its display name, author, version and wemx_version, where wemx_version specifies the wemx version this service supports.

## Config

Your service might require data that has to be set by the administrator such as API key, hostname, port or other information. The setConfig method allows you to dynamically create a form the admin of the panel has to enter. Set it to an empty array if you don't wish to define any settings.

```php
    /**
     * Define the default configuration values required to setup this service
     * i.e host, api key, or other values. Use Laravel validation rules for
     *
     * Laravel validation rules: https://laravel.com/docs/10.x/validation
     *
     * @return array
     */
    public static function setConfig(): array
    {
        return [];
    }
```

Here are some examples of how you can create config values for your Service. The key should be unique to your service.

For the key field, you should use the lower name of your service, followed by the key i.e example::hostname where example is the same of the service, and hostname is the key

```php
    /**
     * Define the default configuration values required to setup this service
     * i.e host, api key, or other values. Use Laravel validation rules for
     *
     * Laravel validation rules: https://laravel.com/docs/10.x/validation
     *
     * @return array
     */
    public static function setConfig(): array
    {
        return [
            [
                "key" => "example::username",
                "name" => "Panel Username",
                "description" => "Username of an administrator on Example Panel",
                "type" => "text",
                "default_value" => "admin",
                "rules" => ['required'], // laravel validation rules
            ],
            [
                "key" => "encrypted::example::password",
                "name" => "example Password",
                "description" => "Password of an administrator on Example Panel",
                "type" => "password",
                "rules" => ['required'], // laravel validation rules
            ],
        ];
    }
```

In case you want to store passwords, API keys or other sensitive data, you can add "encrypted" before the key i.e. encrypted::example::hostname where example is the same of the service, and hostname is the key

This will encrypt the data entered by users in the form, and store it in the database as encrypted. At retrieval, the data is decrypted.

To retrieve the config values, you can use the Settings class or the global function settings()
```php
# Retrieve username
settings('example::username');

# You can pass another param to default
settings('example::username', 'root');
```

You can use the settings class for more complex operations
```php
use App\Models\Settings;

# Get a key
Settings::get('example::hostname', '127.0,0,1');

# Check if a key exists
Settings::has('example::hostname');

# Programmatically store a key value
Settings::put('encrypted::example::password', 'SuperSecure');

# Delete a key
Settings::forget('example::hostname');
```

## Package Config

The package config allows you to define the fields required for the service. It can also be used to set optional params. It uses the same structure as the config above but for key, rather than making it unique per service, you make it unique per package.

```php
    /**
     * Define the default package configuration values required when creating
     * new packages. i.e maximum ram usage, allowed databases and backups etc.
     *
     * Laravel validation rules: https://laravel.com/docs/10.x/validation
     *
     * @return array
     */
    public static function setPackageConfig(Package $package): array
    {
        return 
        [
            [
                "key" => "memory",
                "name" => "Memory in MB",
                "description" => "Allowed memory in MB",
                "type" => "number",
                "default_value" => 1024, # (optional)
                "rules" => ['required'],
            ],
            [
                "key" => "disk_space",
                "name" => "Disk Space in MB",
                "description" => "Allowed disk space in MB",
                "type" => "number",
                "default_value" => 20000, # (optional)
                "rules" => ['required'],
            ]
        ];
    }
```

You can retrieve package settings using
```php
$package->data('disk_space');
```

## Checkout Config

The checkout config are the form fields displayed to the buyer at checkout. It uses the same structure as the configs above.

```php
    /**
     * Define the default package configuration values required when creatig
     * new packages. i.e maximum ram usage, allowed databases and backups etc.
     *
     * Laravel validation rules: https://laravel.com/docs/10.x/validation
     *
     * @return array
     */
    public static function setPackageConfig(Package $package): array
    {
        return 
        [
            [
                "key" => "location",
                "name" => "Server Location ",
                "description" => "Where do you want us to deploy your server?",
                "type" => "select",
                "options" => [
                  "US" => "United States",
                  "CA" => "Canada",
                  "DE" => "Germany",
                ],
                "default_value" => "CA",
                "rules" => ['required'],
            ],
            // add more input fields
        ];
    }
```

You can retrieve checkout settings using
```php
# in create, upgrade, suspend, unsuspend and terminate using
$this->order->option('location');

# or using $order variable
$order = $this->order;
$order->option('location');
```

## Create

The most important function inside a Service is `create()`. Essentially, this function is called when a new order is created.

For instance, if you're selling web hosting packages whenever a package is purchased, you should use the create function to send an API request or call a function to create that webserver. Below is an example using this analogy.

The Http class is a Laravel class that makes sending API requests and other requests much easier. To use it, include `use Illuminate\Support\Facades\Http;` in the top of the file under namespace
https://laravel.com/docs/10.x/http-client

```php
    /**
     * This function is responsible for creating an instance of the
     * service. This can be anything such as a server, vps or any other instance.
     *
     * @return void
     */
    public function create(array $data = [])
    { 
    	$package = $this->order->package;
        $user = $this->order->user;
        $order = $this->order;
      
        $response = Http::post('http://example.com/api/servers/create', [
      	    'username' => $user->username,
            'memory' => $package->data('memory'),
            'disk_space' => $package->data('disk_space'),
            'cpu_limit' => $package->data('cpu_limit'),
        ]);
      
        if($response->failed()) {
            // handle failed response
        }
      
        // store the data inside the orders data
        // so that it can be accessed later
     	$order->update(['data' => $response->json()]);
    }

```

## Suspend

The suspend function is called whenever an order is suspended either by the schedular in an event the user has not paid on time or if an administrator suspends a server manually.

You should handle this function call appropriately by adding a method.

```php
    /**
     * This function is responsible for suspending an instance of the
     * service. This method is called when a order is expired or
     * suspended by an admin
     *
     * @return void
    */
    public function suspend(array $data = [])
    { 
        $response = Http::post('http://example.com/api/servers/suspend', [
            'server_id' => $this->order->data('server_id'),
        ]);
      
        if($response->failed()) {
            // handle failed response
        }
    }

```

## Unsuspend

The unsuspend function is called whenever an order is unsuspended, for example when a client pays an overdue invoice.

You should handle this function call appropriately by adding a method.

```php
    /**
     * This function is responsible for unsuspending an instance of the
     * service. This method is called when a order is activated or
     * unsuspended by an admin
     *
     * @return void
    */
    public function unsuspend(array $data = [])
    { 
      $response = Http::post('http://example.com/api/servers/unsuspend', [
      		'server_id' => $this->order->data('server_id'),
      ]);
      
      if($response->failed()) {
      		// handle failed response
      }
    }

```

## Terminate

The terminate funtion is supposed to completely erase or delete a service. This function is typically called when an invoice has not been paid for a prolonged period of time.

You should handle this function call appropriately by adding a method.

```php
    /**
     * This function is responsible for deleting an instance of the
     * service. This can be anything such as a server, vps or any other instance.
     *
     * @return void
    */
    public function terminate(array $data = [])
    { 
      $response = Http::post('http://example.com/api/servers/delete', [
      		'server_id' => $this->order->data('server_id'),
      ]);
      
      if($response->failed()) {
      		// handle failed response         
      }
    }

```

## Upgrade (optional)

The upgrade function is optional. If your service does not support upgrading or downgrading delete this function. This function should try to update the values of a service as they are set for each order for the new package in the package config

```php
    /**
     * This function is responsible for upgrading or downgrading
     * an instance of this service. This method is optional
     * If your service doesn't support upgrading, remove this method.
     * 
     * Optional
     * @return void
    */
    public function upgrade(Package $oldPackage, Package $newPackage)
    {
        $server_id = $this->order->data['id'];
        $response = Http::post("http://example.com/api/servers/{$server_id}/update", [
            'memory' => $newPackage->data('memory'),
            'disk_space' => $newPackage->data('disk_space'),
            'cpu_limit' => $newPackage->data('cpu_limit'),
        ]);
    }
```

## Permissions (optional)

The permissions functions allows you to protect custom routes or pages for your service by checking if a member / subuser has the corressponding permission to perform a certain action or to access a page. For instance, if your service has custom routes defined for clients, the key of the permission is the name of the route.

Take a look at this custom route that allows clients to start their server. It's important that your route(s) contains the `{order}` parameter as shown in the example below

```php
 Route::get('/start', [Service::class, 'startServer'])->name('proxmox.server.start');
```

```php
    /**
     * Define custom permissions for this service
     *
     * key: Route name or page name
     * value: Short description of what the permission does
     *
     * @return array
     */
    public static function permissions(): array
    {
        return [
            'proxmox.server.start' => [
                'description' => 'Permission to start a Proxmox VM from the dashboard',
            ],
            // define more permissions
        ];
    }
```

## Service buttons (optional)

Service buttons are a set of custom buttons that appear on the edit page of orders. You can optionally include the function below if you wish to add custom buttons.

```php
    /**
     * Define buttons shown at order management page
     *
     * @return array
     */
    public static function setServiceButtons(Order $order): array
    {
        return [
            [
                "name" => "Login to Hestia",
                "color" => "primary",
                "href" => 'https://'. settings('hestia::hostname') . ':' . settings('hestia::port'),
                "target" => "_blank", // optional
                "onclick" => "alert('Hello!')", // optional execute javascript
            ],
            // add more buttons
        ];    
    }
```

## Sidebar buttons (optional)

Sidebar buttons are a set of custom buttons that appear on the sidebar of orders. You can optionally include this function to define them otherwise delete this method

```php
    /**
     * Define sidebar buttons
     *
     * @return array
     */
    public static function setServiceSidebarButtons(Order $order): array
    {
        return [
            [
                "name" => "Domains",
                "icon" => "<i class='bx bx-globe' ></i>",
                "href" => route('hestia.domains.view', $order->id)
            ],
            [
                "name" => "Mail",
                "icon" => "<i class='bx bxs-envelope' ></i>",
                "href" => route('hestia.mail.view', $order->id)
            ],
            [
                "name" => "Databases",
                "icon" => "<i class='bx bxs-data' ></i>",
                "href" => route('hestia.databases.view', $order->id)
            ],
            // more items below
        ];    
    }
```
## Test Connection (optional)

This method is optional and not required to include if your service does not support it.

The Test Connection method allows you to define a function to test the connection between WemX and an API. In this method, you will need to specify a method that attempts to connect to an API. 

```php
    /**
     * Test API connection
    */
    public static function testConnection()
    {
        try {
            // try to get list of packages through API request
            self::api()->getModuleUser()->listUserPackages();
        } catch(\Exception $error) {
            // if try-catch fails, return the error with details
            return redirect()->back()->withError("Failed to connect to Hestia. <br><br>[Hestia] {$error->getMessage()}");
        }

        // if no errors are logged, return a success message
        return redirect()->back()->withSuccess("Successfully connected with Hestia");
    }
```

## Service Provider

A service provider in Laravel is a file that is auto-loaded. In this file you can register views, routes, translations, configs, migrations and much more.

You can find the Service provider in `Providers/YourNameServiceProvider.php`

Here are some properties from a Service Provider, you can set views to true if your service needs custom views. After setting it to true, create the views folder `Resources/views` in your service.

```php
    /**
     * Register routes (Routes)
     * 
     * @return bool
     */
    protected $routes = false;

    /**
     * Register views (Resources/views)
     * 
     * @return bool
     */
    protected $views = false;
```

In this file, you will find properties that you can set to true if you wish to include them such as views, routes, config etc.

# Form Builder

WemX uses consistent structure for the forms for the config, package config and checkout config. Below you can find useful information about different types of methods.

## Rules

Rules allow you to sanitize data before storing it inside the database. For example, if you have a form field that requires an IP address, you can enforce that by adding rules to the field: `['required', 'ip']`

If you want to make a field optional, you can use `nullable` - check the link below for all the available rules. You can also create custom rules and specify them with `['required', new \App\Rules\CustomRule]`

```php
            [
                "key" => "domain",
                "name" => "Website Domain",
                "description" => "Please enter a domain",
                "type" => "text",
                "rules" => ['required', new \App\Rules\ValidDomain], // laravel validation rules
            ]
```

Laravel validation rules: https://laravel.com/docs/10.x/validation#available-validation-rules

## Types

You can use different types for form depending on the data being stored, here's a few examples:

### Text


```php
            [
                "key" => "Username",
                "name" => "Panel Username",
                "description" => "Username of an administrator on Example Panel",
                "type" => "text",
                "default_value" => "admin",
                "rules" => ['required'], // laravel validation rules
            ]
```

### Number

```php
            [
                "key" => "memory",
                "name" => "Memory in MB ",
                "description" => "Allowed memory in MB",
                "type" => "number",
                "default_value" => 1024, # (optional)
                "rules" => ['required'],
            ]
```

### Select / Dropdown

The select type required the parameter "options"

```php
            [
                "key" => "country",
                "name" => "Country ",
                "description" => "Which country are you located in?",
                "type" => "select",
                "options" => [
                    "US" => "United States",
                    "CA" => "Canada",
                    "DE" => "Germany",
                ],
                "default_value" => "CA",
                "rules" => ['required'],
            ]
```

To allow multiple values to be selected, you can add `"multiple" => true,`

### Bool

A bool is a true or false object. Bool type uses a checkbox
```php
            [
                "key" => "is_enabled",
                "name" => "Enable Option ",
                "description" => "Do you want to enable this option?",
                "type" => "bool",
                "rules" => ['required', 'boolean'],
            ]
```





