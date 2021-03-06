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

##### PCRE, Lua & LuaJIT
Install Lua 5.1 (the version that runs in OpenResty) Perl Compatible Regular Expressions & LuaJIT

```bash
$ brew install lua51 luajit pcre
````
Note down the installed version, you'll need this information in the next step.


##### Luarocks
Install Lua's package manager.

Luarocks is installed by default when installing Lua 5.2 with Homebrew, but since we're using Lua 5.1 (for compatibility with OpenResty) we need to install it manually. To do so, download the latest Luarocks from the [official repository](https://github.com/keplerproject/luarocks/releases) (at the time of the writing, it's v2.2.2), untar the file and then install it:

```bash
$ ./configure
$ make build
$ make install
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
    mysql  Ver 14.14 Distrib 5.6.21, for osx10.10 (x86_64) using  EditLine wrapper
    ```

    You may optionally want to start it automatically upon login, if so:

    ```bash
    $ ln -sfv /usr/local/opt/mysql/*.plist ~/Library/LaunchAgents
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

    You may need to specify where to find the MySQL header files, like so:

    ```bash
    luarocks install luadbi-mysql MYSQL_INCDIR=/usr/local/opt/mysql/include/mysql
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
    $ psql --ver
    psql (PostgreSQL) 9.3.5
    ```

    You may optionally want to start it automatically upon login, if so:

    ```bash
    $ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
    ```

    To launch it immediately without rebooting your box:

    ```bash
    $ launchctl load ~/Library/LaunchAgents/homebrew.mxcl.postgresql.plist
    ```

    Try to login with the default `postgres` role:

    ```bash
    $ psql -U postgres
    psql (9.3.5)
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

* Install LuaDBI

    Now that you have MySql installed with the header files, proceed to install LuaDBI for PostgreSql:

    ```bash
    luarocks install luadbi-postgresql
    ```

    > This additional driver is not needed by OpenResty since it has its own embedded driver. LuaDBI is used by Gin to run the console and the migrations.


##### OpenResty
Gin runs on [OpenResty](http://openresty.org/), a customized bundle of [Nginx](http://nginx.org/).

 * Install OpenResty

    Go to [OpenResty](http://openresty.org/#Download) in the downloads section and grab the latest release. Then:

    ```bash
    $ wget http://openresty.org/download/ngx_openresty-{VERSION}.tar.gz
    $ tar -zxvf ngx_openresty-{VERSION}.tar.gz
    $ cd ngx_openresty-{VERSION}/
    $ ./configure \
        --with-cc-opt="-I/usr/local/Cellar/pcre/{PCRE-VERSION}/include" \
        --with-ld-opt="-L/usr/local/Cellar/pcre/{PCRE-VERSION}/lib" \
        --with-luajit
    $ make
    $ make install
    ```

    > Note: Ensure to properly substitute `{VERSION}` and `{PCRE-VERSION}` with OpenResty and PCRE versions respectively.

    If you plan to use PostgreSQL, then you need to have PostgreSQL already installed and
    the configure step needs to provide the additional option `--with-http_postgres_module`:

    ```bash
    $ ./configure \
        --with-cc-opt="-I/usr/local/Cellar/pcre/{PCRE-VERSION}/include" \
        --with-ld-opt="-L/usr/local/Cellar/pcre/{PCRE-VERSION}/lib" \
        --with-http_postgres_module \
        --with-luajit
    ```

 * Configure the PATH to OpenResty

    Assuming you have installed OpenResty into `/usr/local/openresty` (this is the default), make the nginx executable of your OpenResty installation available in the PATH environment, appending at the end of `~/.bash_profile`:

    ```bash
    export PATH=/usr/local/openresty/nginx/sbin:$PATH
    ```

 * Check to see if OpenResty got successfully installed (you might need to `source ~/.bash_profile` or just restart the terminal for this to work):

    ```bash
    $ nginx -v
    nginx version: openresty/1.9.3.1
    ```


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

GIN v0.1.4, a JSON-API web framework.

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
