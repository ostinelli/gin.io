---
layout: docs
title: GIN.IO | API Console
---


# Routes

Routes for your application are defined in the file `./config/routes.lua`. A very simple route file that exposes a `user` resource looks like this:

```lua
-- define version
local v1 = Routes.version(1)

-- define routes
v1:GET("/users", { controller = "users", action = "index" })
v1:POST("/users", { controller = "users", action = "create" })
v1:GET("/users/:id", { controller = "users", action = "show" })
v1:PATCH("/users/:id", { controller = "users", action = "update" })
v1:DELETE("/users/:id", { controller = "users", action = "delete" })
```

Let's see what these lines do. The first line specifies a container for version `1` of your routes:

```lua
local v1 = Routes.version(1)
```

> Gin require an integer number to define a version, as these routes are defined for major versions only. If you need to provide support for minor versions, it is possible to do so in [controllers](/docs/controllers.html).

The other lines specify HTTP action routes specific to version `1`. For example:

```lua
v1:GET("/users", { controller = "users", action = "index" })
```

This line routes all HTTP `GET` requests to `/users` to the action `index` of the controller `users_controller.lua`, whose complete path is `./app/controllers/1/users_controller.lua` (see [API Versioning](/docs/api_versioning.html) for additional information on controller paths).

If you need to provide support for multiple major versions, all you have to do is add multiple versions and build routes out of them, like so:

```lua
-- define version 1
local v1 = Routes.version(1)

-- define routes
v1:GET("/users", { controller = "users", action = "index" })
v1:POST("/users", { controller = "users", action = "create" })

-- define version 2
local v2 = Routes.version(2)

-- define routes
v2:GET("/users", { controller = "users", action = "index" })
v2:POST("/users", { controller = "users", action = "create" })
v2:GET("/users/:id", { controller = "users", action = "show" })
```

Requests to version `1` of the API will be served by controllers in the `./app/controllers/1` directory, requests to version `2` from the ones in the `./app/controllers/2` directory.


##### Named routes

Named routes are supported, for instance:

```lua
v1:GET("/users/:id", { controller = "users", action = "show" })
```

This line routes all HTTP `GET` requests to `/users/{id}` to the action `show` of the controller `users_controller.lua`.

Inside of the controller, the named parameter `:id` will be available as `self.params.id`. For example, in a `GET` request to `/users/gin`, the value of `self.params.id` in the controller's action `show` will be `gin`.

Nested named routes are possible, for instance:

```lua
v1:GET("/users/:user_id/messages/:id", { controller = "messages", action = "show" })
```

This line will expose in the `show` action of the `messages_controller.lua` the two nested routes parameters `self.params.user_id` and `self.params.id`.


##### Patterns

[Lua patterns](http://www.lua.org/pil/20.2.html) are supported. For instance:

```lua
v1:GET("/pages/(.*)", { controller = "pages", action = "show" })

```

This will match all HTTP `GET` requests done to `/pages/*`. The captured portion of the route will be available in the `show` action of the controller `pages_controller.lua` as `self.params[1]`.

For example, in a `GET` request to `/pages/home`, the value of `self.params[1]` in the controller's action `show` will be `home`.
