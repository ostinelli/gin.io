---
layout: docs
title: RALIS.IO | Install
---

# Install

### On OSX
These instructions allow you to install Ralis on a OSX developer machine.

##### Homebrew
The instructions here below will assume that you have [HomeBrew](http://brew.sh/) installed.
If you don't have it, you can easily install it by running:

```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

##### OpenResty
Ralis runs on [OpenResty](http://openresty.org/), a customized bundle of [Nginx](http://nginx.org/).

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

##### Lua & Luarocks
Lua and a lua package manager.

```bash
$ brew install lua luarocks
```

##### Ralis
```bash
$ git clone git@github.com:ostinelli/ralis.git
$ cd ralis
$ luarocks make
```

# Test installation

Check to see if OpenResty got successfully installed:

```bash
$ nginx -v
nginx version: ngx_openresty/1.2.8.6
```

Check if the Ralis client got correctly installed:

```bash
$ ralis
RALIS v0.1-rc1, a RESTful API web framework. Usage: ralis COMMAND [ARGS]

The available ralis commands are:
 new [name]      Create a new Ralis application
 start           Starts the Ralis server
 stop            Stops the Ralis server
```

If you can see both of these, consider your installation of Ralis a success!

You can now proceed to [Getting Started](/docs/getting_started.html) to create your first Ralis app.
