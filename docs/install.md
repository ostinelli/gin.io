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

Go to a workspace directory:

```bash
$ cd ~/workspace
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

Create a new Ralis app:

```bash
$ ralis new demo
Creating app demo...
  created file demo/priv/test-server.key
  created file demo/priv/development-server.key
  created file demo/app/models/.gitkeep
  created file demo/priv/test-server.crt
  created file demo/config/settings.lua
  created file demo/priv/development-server.crt
  created file demo/spec/spec_helper.lua
  created file demo/spec/models/.gitkeep
  created file demo/config/nginx.conf
  created file demo/spec/controllers/1/pages_controller_spec.lua
  created file demo/lib/.gitkeep
  created file demo/config/application.lua
  created file demo/app/controllers/1/pages_controller.lua
  created file demo/config/database.lua
  created file demo/config/initializers/errors.lua
  created file demo/config/routes.lua
```

A new `hello world` app has been created for you. CD into the `demo` directory:

```bash
$ cd demo
```

Ensure the demo tests can be run:

```bash
$ busted

1 success / 0 failures / 0 pending : 0.039959 seconds.
```

Run the demo server:

```bash
$ ralis start
Ralis app in development was succesfully started on port 7200.
```

Open up the browser and point it to [https://localhost:7200/](https://localhost:7200/). You should see an error message:

```javascript
{
    code: 101,
    message: "Invalid Accept header format."
}
```
This is expected behavior, and you can consider your installation of Ralis a success!

For the curious: Ralis uses HTTP headers to implement API versioning. By accessing your Ralis `demo` application directly from your browser
you're not setting the proper headers, and are therefore seeing this error.

If you actually want to see Ralis reply to you with the `hello world` of your `demo` application, open up the very convenient [Ralis API console](/console.html) by accessing the URL [https://localhost:7200/ralisconsole](https://localhost:7200/ralisconsole) and hitting the `HIT` button.
The console is a developer tool that allows you to play around with your API in a web browser.
