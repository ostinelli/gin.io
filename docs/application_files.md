---
layout: docs
title: GIN.IO | Application Files
---


# Application Files

A greenfield `demo` application has the following structure:

```
-- demo
    |-- app
        |-- controllers
            |-- 1
                pages_controller.lua
        |-- models
            .
    |-- config
        application.lua
        errors.lua
        nginx.conf
        routes.lua
        settings.lua
    |-- db
        |-- migrations
            .
        |-- schemas
            .
        mysql.lua
    |-- lib
        .
    |-- spec
        |-- controllers
            |-- 1
                pages_controller_spec.lua
        |-- models
            .
        spec_helper.lua

```

Let's look what these files and directories contain.

##### ./app
The `app` directory is the main container for all the code of your application. It contains all the controllers (in the `controllers` subdirectory) and all of the models (in the `models` subdirectory).

###### ./app/controllers
The `controllers` subdirectory contains the directories of all of API major versions that are supported by your application. The greenfield application supports only a single major version (1), hence it only contains the directory `1`. You can read more about API versioning [here](/docs/api_versioning.html).

Inside the versioned directories, you can find all of the controllers. The greenfield application has a single controller for version 1: `pages_controller.lua`, which basically just responds with an `hello world` example on `/`. More on controllers [here](/docs/controllers.html).

###### ./app/models
The `models` subdirectory contains all your models. More on models [here](/docs/models.html).

##### ./config
The `config` directory contains all the configuration files of your app, and allows you to define your own initializers.

###### `application.lua`
This file is currently used only to define your application name and version. The application name is used to define the `Accept` header used in [API versioning](/docs/api_versioning.html). Here's what a normal file looks like:

```lua
Application = {
    name = "demo",
    version = "0.0.1"
}
```

###### `errors.lua`
A single entry point that defines all of the errors that your application can raise. More on errors can be read [here](/docs/errors.html).


###### `nginx.conf`
This file will be used to dinamically generate the file that will be used by an instance of OpenResty's `nginx`. It contains the necessary code to make Gin work, but can be customized at will like a normal `nginx.conf` file (for example, to add SSL support).

```
pid tmp/{{ "{{GIN_ENV" }}}}-nginx.pid;

# This number should be at maxium the number of CPU on the server
worker_processes 4;

events {
    # Number of connections per worker
    worker_connections 4096;
}

http {
    # use sendfile
    sendfile on;

    # Gin initialization
    {{ "{{GIN_INIT" }}}}

    server {
        # List port
        listen {{ "{{GIN_PORT" }}}};

        # Access log with buffer, or disable it completetely if unneeded
        access_log logs/{{ "{{GIN_ENV" }}}}-access.log combined buffer=16k;
        # access_log off;

        # Error log
        error_log logs/{{ "{{GIN_ENV" }}}}-error.log;

        # Gin runtime
        {{ "{{GIN_RUNTIME" }}}}
    }
}
```

 You can see that it contains 2 parameters, which take their values defined in the `settings.lua` file (see here below):

 * `{{ "{{GIN_ENV" }}}}`: it contains the Gin environment the server is run on, such as `development`, `production`, etc
 * `{{ "{{GIN_PORT" }}}}`: ` {{GIN_PORT}}`: the `nginx` port


It also contains two Gin helpers:

 * `{{ "{{GIN_INIT" }}}}`, which sets the Gin initialization code
 * `{{ "{{GIN_RUNTIME" }}}}`, which sets the Gin runtime code

###### `routes.lua`
This is where your application's versioned routes get defined. You can read more on routes [here](/docs/routes.html).

###### `settings.lua`
This file defines the Gin environments, and the settings to be used for every one of them. By default, Gin defines three environments: `development`, `test` and `production`. This is what a basic file looks like:

```lua
local Settings = {}

Settings.development = {
    code_cache = false,
    port = 7200,
    expose_api_console = true
}

Settings.test = {
    code_cache = true,
    port = 7201,
    expose_api_console = false
}

Settings.production = {
    code_cache = true,
    port = 80,
    expose_api_console = false
}

return Settings

```

The three Gin parameters that can be specified in every environment are:

* `code_cache`: whether to cache code or not. If set to `false`, the code will reload on every page request. This is to be used for development purposes only, as it has a considerable performance impact. Set this to `true` on production environments. This value defaults to `false` for the `development` environment, to `true` for all the other environments.
* `port`: the port to be used by `nginx`. This value default to `7200` for the `development` environment, to `7201` for the `test` environment and to `80` for all of the other environments (including `production`).
* `expose_api_console`: if set to `true`, expose the [API Console](/docs/api_console.html). Defaults to `true` for the `development` environment, to `false` for all of the other environments.

You may also specify your own custom settings in here. All of the setting parameters specified in this file will be available inside your application: `Gin.settings` will return the ones that correspond to the environment you are running the server in.
For example, `Gin.settings.port` returns the port the server is running on.


##### ./db

This directory is used to namespace everything related to your databases.

###### `mysql.lua`
This generated file contains an example on how to define a connection to a `mysql` database. Please refer to [models](/docs/models.html) on how to use this file.

Gin supports having multiple databases in your application: all you have to do is add multiple files like this one, and reference them in your models or where needed. You may also consider setting connections to [Redis](http://redis.io/), [RabbitMQ](http://www.rabbitmq.com/) or other types of datastores.

###### ./db/migrations
This directory contains all of your SQL migrations. Read more about migrations [here](/docs/migrations.html).

###### ./db/schemas
This directory contains a Lua representation of the current schema of your SQL databases, after having run the migrations.


##### ./lib
This directory may be used to put any library files you might need to require from your code.

##### ./spec
Gin comes with test helpers to test drive your code. By convention, the subdirectories and filenames match the ones defined in the `./app` directory, with one caveat: all of the test files must have their name end in `*_spec.lua`.

The `spec_helper.lua` file is a helper file that needs to be included in all of your tests to be able to access test runners. You may however also use it to define your own custom test helpers. More on tests [here](/docs/testing.html).
