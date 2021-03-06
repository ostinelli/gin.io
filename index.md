---
layout: home
title: GIN.IO
---


# GIN JSON-API framework

Gin is an JSON-API framework, currently in its early stage.

It has been designed to allow for fast development, TDD and ease of maintenance.

Gin is helpful when you need an extra-boost in performance and scalability, as it runs embedded in a packaged version of nginx
called [OpenResty](http://openresty.org/) and it's entirely written in [Lua](http://www.lua.org/).
For those not familiar with Lua, don't let that scare you away: Lua is really easy to use, very fast and simple to get started with.

For instance, this is what a simple Gin controller looks like:

```lua
local InfoController = {}

function InfoController:whoami()
    return 200, { name = 'gin' }
end

return InfoController
```

When called, this returns an HTTP `200` response with body:

```json
{
    "name": "gin"
}
```

#### Features

Gin already provides:

 * [API Versioning](/docs/api_versioning.html) embedded in the framework
 * [Routes](/docs/routes.html) with named and pattern routes support
 * [Controllers](/docs/controllers.html)
 * [Models](/docs/models.html) and a MySql ORM
 * [Migrations](/docs/migrations.html) for SQL engines
 * [Test helpers](/docs/testing.html) and wrappers
 * Simple [error](/docs/errors.html) raising and definition
 * Support for multiple databases in your application
 * An embedded [API Console](/docs/api_console.html) to play with your API
 * A client to create, start and stop your applications

Get started now! Start reading our [docs](/docs/install.html) or go for our simple [tutorial](tutorial.html).
