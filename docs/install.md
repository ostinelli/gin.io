---
layout: docs
title: GIN.IO | Install
---


# Install

### On OSX
These instructions allow you to install Gin on a OSX developer machine.

##### Homebrew
The instructions here below describe the installation flow using [HomeBrew](http://brew.sh/).
If you don't have it installed, you can easily install it by running:

```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

##### OpenResty
Gin runs on [OpenResty](http://openresty.org/), a customized bundle of [Nginx](http://nginx.org/).

 * Install Perl Compatible Regular Expressions & LuaJIT

    ```bash
    $ brew install pcre luajit
    ````
    Note down the installed version, you'll need this information in the next step.

 * Install OpenResty

    Go to [OpenResty](http://openresty.org/#Download) in the downloads section and grab the latest Mainline release. Then:

    ```bash
    $ wget http://openresty.org/download/ngx_openresty-VERSION.tar.gz
    $ tar zxvf ngx_openresty-VERSION.tar.gz
    $ cd ngx_openresty-VERSION/
    $ ./configure \
        --with-cc-opt="-I/usr/local/Cellar/pcre/PCRE-VERSION/include" \
        --with-ld-opt="-L/usr/local/Cellar/pcre/PCRE-VERSION/lib" \
        --with-http_postgres_module \
        --with-luajit
    $ make
    $ make install
```

 * Configure the PATH to OpenResty

    Assuming you have installed OpenResty into `/usr/local/openresty` (this is the default), make the nginx executable of your OpenResty installation available in the PATH environment, appending at the end of `~/.bash_profile`:

    ```bash
    export PATH=/usr/local/openresty/nginx/sbin:$PATH
    ```

 * Check to see if OpenResty got successfully installed

    ```bash
    $ nginx -v
    nginx version: ngx_openresty/1.4.3.3
    ```

##### Lua & Luarocks
Install Lua and its package manager.

```bash
$ brew install lua luarocks
```


##### MySql
If you're planning to use MySql, you'll need to have an installed MySql copy together with its header files so that [LuaDBI](https://code.google.com/p/luadbi/) can be compiled.

* Install MySql server and its header files

    ```bash
    $ brew install mysql
    ```

    Ensure that it is installed:

    ```bash
    $ mysql --version
    mysql  Ver 14.14 Distrib 5.6.13, for osx10.9 (x86_64) using  EditLine wrapper
    ```

    You may optionally want to start it automatically upon login, if so:

    ```bash
    $ cp /usr/local/Cellar/mysql/MYSQL-VERSION/homebrew.mxcl.mysql.plist ~/Library/LaunchAgents/
    ```

    To launch it immediately without rebooting your box:

    ```bash
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mysql.plist
    ```


* Install LuaDBI

    Now that you have MySql installed with the header files, proceed to install LuaDBI for MySql:

    ```bash
    luarocks install luadbi-mysql
    ```

    > This additional driver is not needed by OpenResty since it has its own embedded driver. LuaDBI is used by Gin to run the console and the migrations.


##### PostgreSql
If you're planning to use PostgreSql, you'll need to have an installed PostgreSql copy together with its header files so that [LuaDBI](https://code.google.com/p/luadbi/) can be compiled.

* Install PostgreSql server and its header files

    ```bash
    $ brew install postgresql
    ```

    Ensure that it is installed:

    ```bash
    $ $ psql --ver
    psql (PostgreSQL) 9.3.1
    ```

    Try to login with the default `postgres` role:

    ```bash
    $ psql -U postgres
    psql (9.3.1)
    Type "help" for help.

    postgres=#
    ```

    If previous instruction returns an error because your database isn't initialized yet, just issue the command:

    ```bash
    initdb /usr/local/var/postgres
    ```

    If trying to login with the default user returns the error `psql: FATAL:  role "postgres" does not exist`, then simply create the default role:

    ```bash
    $ createuser -s postgres
    ```
    You may optionally want to start it automatically upon login, if so:

    ```bash
    $ cp /usr/local/Cellar/mysql/POSTGRESQL-VERSION/homebrew.mxcl.postgresql.plist ~/Library/LaunchAgents/
    ```

    To launch it immediately without rebooting your box:

    ```bash
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
    ```


* Install LuaDBI

    Now that you have MySql installed with the header files, proceed to install LuaDBI for PostgreSql:

    ```bash
    luarocks install luadbi-postgresql
    ```

    > This additional driver is not needed by OpenResty since it has its own embedded driver. LuaDBI is used by Gin to run the console and the migrations.

##### Gin
To install the latest Gin, issue the command:

```bash
$ luarocks install --server=http://gin.io/repo gin
```

Check if the Gin client got correctly installed:

```bash
$ gin

           /++++/:
           NNNNNmm
           mNNMNmy
          .dNMMNms`
      .:+sydMMMMMdyo/-`
   `sdmmmNNNMMMMMNmdhhddo
   `dMMNNMMMMMMMMMNdhdMMy
   `dMMMNMMMMdsymMNdhdNMy
   `dMMMNNho-....:shhdNMy
   `dMMdo-......`.``:sNMy
   `dd/........-..````-hy
   `:...--.`----..`````.-
   ``//+-  G  I  N  .+-/`
   `:..-..``-...-.`.`.``-
   `hs-```-./.:`-/`.```os
   `hNms-.``.-.-.````+mmy
   `hNNNNh+.-:---`-smNNmy
   `hNNNMMMNy+.:sdNNNNNmy`
   `dNNNNMMMMMNNNNNNNNNNy`
   `hNNNNNMMMMMMMMNNmNNNy`
    ./osyhhddddddhhyss+:`

GIN v0.1, a JSON-API web framework.

Usage: gin COMMAND [ARGS]

The available gin commands are:
 new [name]             Create a new Gin application
 start                  Starts the Gin server
 stop                   Stops the Gin server
 console                Start a Gin console
 generate migration     Create a new migration
 migrate                Run all migrations that have not been run
 migrate rollback       Rollback one migration
```

If you can see this, consider your installation of Gin a success!

You can now proceed to [Getting Started](/docs/getting_started.html) to create your first Gin app.
