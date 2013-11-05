---
layout: docs
title: ZEBRA.IO | Application Files
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
        |-- initializers
            errors.lua
        application.lua
        nginx.conf
        routes.lua
        settings.lua
    |-- db
        |-- migrations
            .
        |-- schemas
            .
        db.lua
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

###### `nginx.conf`

This file will be used to dinamically generate the file that will be used by an instance of OpenResty's `nginx`. It contains the necessary code to make Zebra work, but can be customized at will like a normal `nginx.conf` file (for example, to add SSL support).

```
worker_processes 1;
pid tmp/{{ "{{ZEBRA_ENV" }}}}-nginx.pid;

events {
    worker_connections 1024;
}

http {
    sendfile on;

    lua_code_cache {{ "{{ZEBRA_CODE_CACHE" }}}};
    lua_package_path "./?.lua;$prefix/lib/?.lua;#{= LUA_PACKAGE_PATH };;";

    server {
        access_log logs/{{ "{{ZEBRA_ENV" }}}}-access.log;
        error_log logs/{{ "{{ZEBRA_ENV" }}}}-error.log;

        listen {{ "{{ZEBRA_PORT" }}}};

        location / {
            content_by_lua 'require(\"zebra.core.router\").handler(ngx)';
        }

        location /zebraconsole {
            {{ "{{ZEBRA_API_CONSOLE" }}}}
        }
    }
}
```

 You can see that it contains 3 parameters, all of which takes values defined in the `settings.lua` file (see here below):

 * `{{ "{{ZEBRA_ENV" }}}}`: it contains the Zebra environment the server is run on, such as `development`, `production`, etc
 * `{{ "{{ZEBRA_CODE_CACHE" }}}}`: can be `on` or `off`
 * `{{ "{{ZEBRA_PORT" }}}}`: ` {{ZEBRA_PORT}}`: the `nginx` port


It also contains one last parameter, not specified in `settings.lua` file: `{{ "{{ZEBRA_API_CONSOLE" }}}}`, a helper to set the code needed to run the [API console](/docs/api_console.html) out of a specific directory (by default, `/zebraconsole`).


###### `routes.lua`
This is where your application's versioned routes get defined. You can read more on routes [here](/docs/routes.html).

###### `settings.lua`
This file defines the Zebra environments, and the settings to be used for every one of them. By default, Zebra defines three environments: `development`, `test` and `production`. This is what a basic file looks like:

```lua
local Settings = {}

Settings.development = {
    code_cache = false,
    port = 7200
}

Settings.test = {
    code_cache = true,
    port = 7201
}

Settings.production = {
    code_cache = true,
    port = 80
}

return Settings
```

The two parameters that need to be specified on every environment are:

* `code_cache`: whether to cache code or not. If set to `false`, the code will reload on every page request. This is to be used for development purposes only, as it has a considerable performance impact. Set this to `true` on production environments. This value defaults to `false` for the `development` environment, to `true` for `test` and `production` environments.
* `port`: the port to be used by `nginx`. This value default to `7200` for the `development` environment, to `7201` for the `test` environment and to `80` for the `production` environment.

You may also specify your own custom settings in here. All of the setting parameters specified in this file will be available inside your application: `Zebra.settings` will return the ones that correspond to the environment you are running the server in.
For example, `Zebra.settings.port` returns the port the server is running on.


###### ./config/initializers
All of the `*.lua` files added here will be run as part of the initialization process. By default, it must contain an `errors.lua` file, a single entry point that defines all of the errors that your application can raise. More on errors can be read [here](/docs/errors.html).

##### ./db

This directory is used to namespace everything related to your databases.

###### `db.lua`
Your database connections can be defined here. Multiple database can be supported. The generated file contains an example on how to define a connection to a `mysql` database (currently, the only RDBMS database with a Zebra ORM). Please refer to [models](/docs/models.html) for how to use this file.

You may also consider setting connections to [Redis](http://redis.io/), [RabbitMQ](http://www.rabbitmq.com/) or other types of datastores.

###### ./db/migrations
This directory contains all of your SQL migrations. Read more about migrations [here](/docs/migrations.html).

###### ./db/schemas
This directory contains a Lua representation of the current schema of your SQL databases, after having run the migrations.


##### ./lib
This directory may be used to put any library files you might need to require from your code.

##### ./spec
Zebra comes with test helpers to test drive your code. By convention, the subdirectories and filenames match the ones defined in the `./app` directory, with one caveat: all of the test files must have their name end in `*_spec.lua`.

The `spec_helper.lua` file is a helper file that needs to be included in all of your tests to be able to access test runners. You may however also use it to define your own custom test helpers.
