---
layout: docs
title: GIN.IO | Other
---

# Other

##### The Gin object
In your application, `Gin` exposes some attributes you might need. After you've required it:

```lua
local Gin = require 'gin.core.gin'
```

 * `Gin.env`: returns the current environment (`development`, `test`, `production`, or any other environment you may be running Gin in)
 * `Gin.version`: returns the current Gin version
 * `Gin.settings`: returns the global settings defined per environment in `./config/settings.lua` (see [Application files](/docs/application_files.html), under `settings.lua`)


##### JSON
If you need to manually encode/decode JSON, you can easily do so by first requiring `cjson`:

```lua
local json = require 'cjson'
```

 * to encode: `json.encode(table)`
 * to decode: `json.decode(string)`

json exposes the [CJSON](http://www.kyne.com.au/~mark/software/lua-cjson.php) class.


##### Running in environments
To run Gin in an environment different than `development` (the default), ensure this environment has been defined where appropriate (at least in `./config/settings.lua`) and run:

```bash
$ GIN_ENV={environment} gin start
```

For instance, to run Gin in production:

```bash
$ GIN_ENV=production gin start
```

To stop:

```bash
$ GIN_ENV={environment} gin stop
```
