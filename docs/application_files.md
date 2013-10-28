---
layout: docs
title: RALIS.IO | Getting Started
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
        database.lua
        nginx.conf
        routes.lua
        settings.lua
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
The `config` directory contains all the configuration files of your app, and allows you to define your own initializers. The configuration files are:

 * `application.lua`: this file is currently used only to define your application name. After creating your app, you shouldn't probably need to change it. The application name is used to define the `Accept` header used in [API versioning](/docs/api_versioning.html).

 * `database.lua`: your database connections can be defined here. Multiple database can be supported. The generated file contains an example on how to define a connection to a `mysql` database (currently, the only RDBMS database with a Ralis ORM). You may also consider setting connections to [Redis](http://redis.io/), [RabbitMQ](http://www.rabbitmq.com/) or other types of datastores.

 * `nginx.conf`: this file will be used to dinamically generate the file that will be used by an instance of OpenResty's `nginx`. It contains the necessary code to make Ralis work, but can be customized at will like a normal `nginx.conf` file (for example, to add SSL support). You can see that it contains 3 parameters, all of which takes values defined in the `settingsl.lua` file (see here below):
    * `{{ "{{RALIS_ENV" }}}}`: it contains the Ralis environment the server is run on, such as `development`, `production`, etc
    * `{{ "{{RALIS_CODE_CACHE" }}}}`: can be `on` or `off`
    * `{{ "{{RALIS_PORT" }}}}`: ` {{RALIS_PORT}}`: the `nginx` port


    It also contains one last parameter, not specified in `settings.lua` file: `{{ "{{RALIS_API_CONSOLE" }}}}`, a helper to set the code needed to run the [API console](/docs/api_console.html) out of a specific directory (by default, `/ralisconsole`).
 * `routes.lua`: this is where your application's versioned routes get defined. You can read more on routes [here](/docs/routes.html).

 * `settings.lua`: this file defines the Ralis environments, and the settings to be used for every one of them. By default, Ralis defines three environments: `development`, `test` and `production`. The two parameters that Ralis needs to run are:
    * `code_cache`: whether to cache code or not. If set to `false`, the code will reload on every page request. This is to be used for development purposes only, as it has a considerable performance impact. Set this to `true` on production environments. This value defaults to `false` for the `development` environment, to `true` for `test` and `production` environments.
    * `port`: the port to be used by `nginx`. This value default to `7200` for the `development` environment, to `7201` for the `test` environment and to `80` for the `production` environment.

    You may also specify your own custom settings in here. All of the setting parameters specified in this file will be available inside your application: `Ralis.settings` will return the ones that correspond to the environment you are running the server in.
    For example, `Ralis.settings.port` returns the port the server is running in.


###### ./config/initializers
All of the `.lua` files added here will be run as part of the initialization process. By default, it must contain a `errors.lua` file, a single entry point that defines all of the errors that your application can raise. More on errors can be read [here](/docs.errors.html).

##### ./lib
This directory may be used to put any library files you might need to require from your code.

##### ./spec
Ralis comes with test helpers to test drive your code. By convention, the subdirectories and filenames match the ones defined in the `./app` directory, with one caveat: all of the test files must have their name end in `_spec.lua`.

The `spec_helper.lua` file is a helper file that needs to be included in all of your tests to be able to access test runners. You may however also use it to define your own custom test helpers.
