---
layout: docs
title: RALIS.IO | Install
---

# Install

### On OSX
These instructions allow you to install Ralis on a OSX developer machine.

##### 1. Homebrew
The instructions here below will assume that you have [HomeBrew](http://brew.sh/) installed.
If you don't have it, you can easily install it by running:

```bash
$ ruby -e "$(curl -fsSL https://raw.github.com/mxcl/homebrew/go)"
```

##### 2. OpenResty
Ralis runs on [OpenResty](http://openresty.org/), a customized bundle of [Nginx](http://nginx.org/).

###### 2.1 Install Perl Compatible Regular Expressions & LuaJIT
```bash
$ brew install pcre luajit
````

###### 2.2 Install OpenResty

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

###### 2.3 Configure the PATH to OpenResty
Assuming you have installed OpenResty into `/usr/local/openresty` (this is the default), make the nginx executable of your OpenResty installation available in the PATH environment, appending at the end of `~/.bash_profile`:

```bash
export PATH=/usr/local/openresty/nginx/sbin:$PATH
```

##### 3. Lua & Luarocks
Lua and a lua package manager.

```bash
$ brew install lua luarocks
```

##### 4. Ralis
```bash
$ git clone git@github.com:ostinelli/ralis.git
$ cd ralis
$ luarocks make
```
