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

    Go to [OpenResty](http://openresty.org/) in the downloads section and grab the latest version number. Then:

    ```bash
    $ wget http://openresty.org/download/ngx_openresty-VERSION.tar.gz
    $ tar zxvf ngx_openresty-VERSION.tar.gz
    $ cd ngx_openresty-VERSION/
    $ ./configure \
        --with-cc-opt="-I/usr/local/Cellar/pcre/PCRE-VERSION/include" \
        --with-ld-opt="-L/usr/local/Cellar/pcre/PCRE-VERSION/lib" \
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
    nginx version: ngx_openresty/1.2.8.6
    ```

##### Lua & Luarocks
Install Lua and its package manager.

```bash
$ brew install lua luarocks
```


##### MySql
If you're planning to use MySql, you'll need to have an installed MySql copy together with its header files so that [LuaSql](http://www.keplerproject.org/luasql/) can be compiled.

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


* Install LuaSql

    Now that you have MySql installed with the header files, proceed to install LuaSql for MySql:

    ```bash
    luarocks install luasql-mysql
    ```


##### Gin
To install Gin, issue the commands:

```bash
$ git clone git@github.com:ostinelli/gin.git
$ cd gin
$ luarocks make
```

Check if the Gin client got correctly installed:

```bash
$ gin

                 ```-`.
             -.-/++.:sdo+.-+:
    `.--::. `s.dh:.++:/MM/`oMms-`
     /`/dhyoydh-dMs .s/oMM/ oMM-/y`
     `s.sMMN-.hd+sh- +:+NMd -MMh.Md:-
       ///+so .+:+:-/.:om:mo:mMy.MMNd
         `-:/:o+/-:`dy. yNym`/MNyMMMM.+`
          :/ss.--:/ `dN:.Mdyh sMMmNMd/m:
         +-od`./ho `.-Md MMhM. yMN-d/yM+
        -N/s+-md.   `+M:oMMMM:  dMo/mNM+
        yydh:NM-   o.sd:N/MMM.  +Ms:sMd:`
       `s-ymsMN     `ddoy mMs   +M:o:msy`
       ://-MsMs  ./+`/M+d dMh   dd`m`dMm
       +:/:dod:yhohN+-Mhd-mMm  oM++s hMy
       s:`ysdsMo.mMMohMhN:NMh /MM-/``NM-
      .s.:+moM+.mMMh/mhsm:Md./MMM-  oMy
      +`.-m:m+-ms+h.hs/sNh:`sMMMM- -MN.
     : ``o.y:/d--N.y/mhy: /mMMMMN `mM/
   ``    `:.o/  .hsy+-`-omh/+MMN:`dM+
  +-  .-   -.` ./`+oydMNs.`/NMh.`dM+
 .h: /sN   .--./  hhs+:/ohNms- :mN:
  `-. .`    o+o. :o-odNds/.  .yMy`
    + ``   +mm:o /h+:.     :yMd-
    `::://syy+.s  o`  `:+hNNs-
          .::::    -/shhs+-

GIN v0.0.1, a JSON-API web framework.

Usage: gin COMMAND [ARGS]

The available gin commands are:
 new [name]             Create a new Gin application
 start                  Starts the Gin server
 stop                   Stops the Gin server
 generate migration     Create a new SQL migration
 migrate                Run all SQL migrations that have not been run
 migrate rollback       Rollback one SQL migration
 console                Start a Gin console

```

If you can see this, consider your installation of Gin a success!

You can now proceed to [Getting Started](/docs/getting_started.html) to create your first Gin app.
