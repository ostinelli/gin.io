---
layout: docs
title: RALIS.IO | Models
---

# Models

In Ralis, you define all of your databases in the config file `./config/database.lua`.

These databases can then be accessed in the various model files located in `./app/models`, where you can implement helper methods to store and query data into and from these databases.

This is an extremely flexible and powerful design, as it allows you to implement multiple Databases support and Message Queues in your application
(for example [Redis](redis.io), [RabbitMQ](http://www.rabbitmq.com/), [NoSQL databases](http://en.wikipedia.org/wiki/NoSQL), but also regular SQL databases such as
[MySQL](http://www.mysql.com/) or [PostgreSQL](http://www.postgresql.org/)).

> In Ralis, a model is therefore not necessarily a SQL database object representation.

However, Ralis does come with a more traditional database helper that allows you to quickly define models that have SQL Object-Relational Mapping functionalities.

> Currently, only a MySQL adapter and ORM are supported.


##### MySQL and its ORM

###### Define a DB and a model

To create a MySQL model with ORM functionalities, we first need to define a new MySQL database in the file `./config/database.lua`, by specifying the `mysql` adapter like so:

```lua
local db = require 'ralis.db.db'

-- Here you can setup your databases that will be accessible throughout your application.
-- First, specify the settings (you may add multiple databases with this pattern):
local DbSettings = {

    development = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "ralis_development",
        user = "root",
        password = "",
        pool = 5
    },

    test = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "ralis_test",
        user = "root",
        password = "",
        pool = 5
    },

    production = {
        adapter = 'mysql',
        host = "127.0.0.1",
        port = 3306,
        database = "ralis_production",
        user = "root",
        password = "",
        pool = 5
    }
}

-- Then initialize your database(s) like this:
DB = db.new(DbSettings[Ralis.env])
```

In Lua, a variable that is not specifically set as `local` is defined globally. Hence, the database variable `DB` defined here above is a global object available
throughout your application.

As you can see, we're defining various database access settings for the three environments `development`, `test` and `production`, and then initializing the
database object with the settings that correspond to the enivornment Ralis is run in (available in `Ralis.env`).

> The newly created object `DB` exposes only two functions: `define` (see here below) and `execute(sql)`. The latter allows you to use the `DB` connection anywhere in your application, including models,
> to perform database queries, by calling `DB:execute(sql)`.

Now that we have a database object, we can define a model `Users`. To do so, create the file `./app/models/users.lua` and enter this code:

```lua
Users = DB:define('users')
```
This defines a model `Users` that corresponds to the database table `users`.

> For the ORM to work properly, the table `users` must have an `AUTO_INCREMENT PRIMARY KEY` named `id`. Please refer to [migrations](/docs/migrations.html) for additional information.

###### Queries

The `Users` model defined here above can now be used to perform standard SQL queries.


 * `Users.new(attrs)`: creates a new user with the passed in attributes. The attributes must correspond to the ones defined in the table `users`. The object is not saved to the database until `save` is called on it.
 This returns the newly created user object. For example:

 ```lua
 local user = Users.new({ first_name = 'ralis' })
 ```

 * `Users.create(attrs)`: creates and saves a new user with the passed in attributes. The attributes must correspond to the ones defined in the table `users`.
 This returns the newly created user object, with the sequence `id` that was returned by MySQL. For example:

 ```lua
 local user = Users.create({ first_name = 'ralis' })
 user.id -- => 1
 ```

 * `Users.all()`: returns all users. For example:

 ```lua
 local users = Users.all()
 ```

 * `Users.where(attrs, options)`: returns users that match the query specified in attributes. `options` is a table that can specify the `limit` and `offset`, and the `order` of the resultset. For example:

 ```lua
 local users = Users.where({ first_name = 'ralis'}, { limit = 5, offset = 10, order = "first_name DESC" } )
 ```

 * `Users.find_by(attrs)`: same as `.where`, but will return only the first match. For example:

 ```lua
 local user = Users.find_by({ first_name = 'ralis'})
 ```

 * `user:save()`: saves a new model instance or updates an existing one. For example:

 ```lua
 local user = Users.new({ first_name = 'ralis' })
 user:save()
 ```

 * `user:delete()`: deletes a model instance. For example:

 ```lua
 local user = Users.find_by({ first_name = 'ralis' })
 user:delete()
 ```

 * `user:class()`: returns a model's instance's class. For example:

 ```lua
 local user = Users.create({ first_name = 'ralis' })
 user:class() -- => Users
 ```
