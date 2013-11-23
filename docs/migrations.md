---
layout: docs
title: GIN.IO | Migrations
---


# Migrations
Gin implements a migration engine to help you handle the evolution of your databases. It currently only supports the SQL databases available in Gin (see [models](/docs/models.html)).

To create a new migration:

```bash
$ gin generate migration
Created new migration file
  db/migrations/20131107134407.lua
```

The migration name is just the timestamp of the moment the migration file was created. Open up the newly generated migration file:

```lua
local SqlMigration = {}

-- specify the database used in this migration (needed by the Gin migration engine)
-- SqlMigration.db = require 'db.mysql'

function SqlMigration.up()
    -- Run your migration
end

function SqlMigration.down()
    -- Run your rollback
end

return SqlMigration
```
To set up a migration, you need to define:

 * the `SqlMigration.db`, which must be one of the available databases, for example the `MySql` database specified in `./db/mysql.lua` (see [application files](/docs/application_files.html) for more info).
 * the `SqlMigration.up()` function, where you basically execute a SQL statement on the specified db. This function will be called by the Gin migration engine when running the migration.
 * the `SqlMigration.down()` function, where you execute a SQL statement on the specified db. This function will be called by the Gin migration engine when rolling back the migration.

The following example migration file shows the creation of a `users` table, and the rollback of the migration which drops the table:

```lua
local SqlMigration = {}

SqlMigration.db = require 'db.mysql'

function SqlMigration.up()
    SqlMigration.db:execute([[
        CREATE TABLE users (
            id int NOT NULL AUTO_INCREMENT,
            first_name varchar(255) NOT NULL,
            last_name varchar(255),
            PRIMARY KEY (id)
        );
    ]])
end

function SqlMigration.down()
    SqlMigration.db:execute([[
        DROP TABLE users;
    ]])
end

return SqlMigration
```

> If you're planning to use Gin's ORM, ensure to have an unique incremental key called `id` on the newly created tables
> (of type `AUTO_INCREMENT PRIMARY KEY` for MySQL, `SERIAL` for PostgreSQL).

The corresponding `SqlMigration.up` method when using PostgreSQL is:

```lua
[...]

function SqlMigration.up()
    SqlMigration.db:execute([[
        CREATE TABLE users (
            id SERIAL,
            first_name varchar(255) NOT NULL,
            last_name varchar(255),
            CONSTRAINT unique_first_name UNIQUE (first_name)
        );
    ]])
end

[...]
```

To run the migration:

```bash
$ gin migrate
Migrating up in development environment
==> Successfully applied migration: 20131107134407
```

> The Gin migration engine will also take care to generate the environment database if it doesn't exist yet.

You will now have the database `demo_development`.

To rollback a single migration:

```bash
$ gin migrate rollback
Rolling back one migration in development environment
<== Successfully rolled back migration: 20131107134407
```
