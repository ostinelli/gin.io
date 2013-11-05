---
layout: docs
title: ZEBRA.IO | Models
---

# Models

In Zebra, you define all of your databases in the file `./db/db.lua`.

These databases can then be accessed in the various model files located in `./app/models`, where you can implement helper methods to store and query data into and from these databases.

This is an extremely flexible and powerful design, as it allows you to implement multiple Databases support and Message Queues in your application
(for example [Redis](redis.io), [RabbitMQ](http://www.rabbitmq.com/), [NoSQL databases](http://en.wikipedia.org/wiki/NoSQL), but also regular SQL databases such as
[MySQL](http://www.mysql.com/) or [PostgreSQL](http://www.postgresql.org/)).

> In Zebra, a model is therefore not necessarily a SQL database object representation.

However, Zebra does come with a more traditional database helper that allows you to quickly define models that have SQL Object-Relational Mapping functionalities.

> Currently, only a MySQL adapter and ORM are supported.


##### MySQL and its ORM

###### Define a DB and a model

To create a MySQL model with ORM functionalities, we first need to define a new MySQL database in the file `./db/db.lua`, by requiring Zebra's SQL helper and by specifying the `mysql` adapter like so:

```lua
local sqldb = require 'zebra.db.sql'

-- Here you can setup your databases that will be accessible throughout your application.
-- First, specify the settings (you may add multiple databases with this pattern), for instance:
local DbSettings = {

    development = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "zebra_development",
        user = "root",
        password = "",
        pool = 5
    },

    test = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "zebra_test",
        user = "root",
        password = "",
        pool = 5
    },

    production = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "zebra_production",
        user = "root",
        password = "",
        pool = 5
    }
}

-- Then initialize your database(s), for instance:
MYSQLDB = sqldb.new(DbSettings[Zebra.env])

```

In Lua, a variable that is not specifically set as `local` is defined globally. Hence, the database variable `DB` defined here above is a global object available
throughout your application.

As you can see, we're defining various database access settings for the three environments `development`, `test` and `production`, and then initializing the
database object with the settings that correspond to the enivornment Zebra is run in (available in `Zebra.env`).

> The newly created object `MYSQLDB` exposes only two functions: `define` (see here below) and `execute(sql)`. The latter allows you to use the `MYSQLDB` connection anywhere in your application, including models,
> to perform database queries, by calling `MYSQLDB:execute(sql)`.

Now that we have a database object, we can define a model `Users`. To do so, create the file `./app/models/users.lua` and enter this code:

```lua
Users = MYSQLDB:define('users')
```
This defines a model `Users` that corresponds to the database table `users`.

> For the ORM to work properly, the table `users` must have an `AUTO_INCREMENT PRIMARY KEY` named `id`. Please refer to [migrations](/docs/migrations.html) for additional information.

###### Queries

The `Users` model defined here above can now be used to perform standard SQL queries.


 * `Users.new(attrs)`: creates a new user with the passed in attributes. The attributes must correspond to the ones defined in the table `users`. The object is not saved to the database until `save` is called on it.
 This returns the newly created user object. For example:

 ```lua
 local user = Users.new({ first_name = 'zebra' })
 ```

 * `Users.create(attrs)`: creates and saves a new user with the passed in attributes. The attributes must correspond to the ones defined in the table `users`.
 This returns the newly created user object, with the sequence `id` that was returned by MySQL. For example:

 ```lua
 local user = Users.create({ first_name = 'zebra' })
 user.id -- => 1
 ```

 * `Users.all(options)`: returns all users. If provided, `options` is a table that can specify the `limit`, the `offset` and the `order` of the resultset. For example:

 ```lua
 local users = Users.all()
 ```

 * `Users.where(attrs, options)`: returns users that match the query specified in attributes. If provided, `options` is a table that can specify the `limit`, the `offset` and the `order` of the resultset. For example:

 ```lua
 local users = Users.where({ first_name = 'zebra'}, { limit = 5, offset = 10, order = "first_name DESC" } )
 ```

 * `Users.find_by(attrs, options)`: same as `.where`, but will return only the first match. If provided, `options` is a table that can specify the `order` of the resultset. For example:

 ```lua
 local user = Users.find_by({ first_name = 'zebra'})
 ```

 * `Users.delete_all(options)`: deletes all users. If provided, `options` is a table that can specify the `limit` of the row count. For example:

 ```lua
 local users = Users.delete_all()
 ```

 * `Users.delete_where(attrs, options)`: deletes all users that match the query specified in attributes. If provided, `options` is a table that can specify the `limit` of the row count. For example:

 ```lua
 local users = Users.delete_where({ first_name = 'zebra'}, { limit = 5 })
 ```

 * `user:save()`: saves a new model instance or updates an existing one. For example:

 ```lua
 local user = Users.new({ first_name = 'zebra' })
 user:save()
 ```

 * `user:delete()`: deletes a model instance. For example:

 ```lua
 local user = Users.find_by({ first_name = 'zebra' })
 user:delete()
 ```

 * `user:class()`: returns a model's instance's class. For example:

 ```lua
 local user = Users.create({ first_name = 'zebra' })
 user:class() -- => Users
 ```
