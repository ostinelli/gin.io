---
layout: docs
title: CARB.IO | Other
---

# Other

##### The Carb object
In your application, `Carb` exposes some attributes you might need.

 * `Carb.env`: returns the current environment (`development`, `test`, `production`, or any other environment you may be running Carb in)
 * `Carb.version`: returns the current Carb version
 * `Carb.settings`: returns the global settings defined per environment in `./config/settings.lua` (see [Application files](/docs/application_files.html), under `settings.lua`)


##### JSON
If you need to manually encode/decode JSON, you can do so by using the global JSON object:

 * to encode: `JSON.encode(table)`
 * to decode: `JSON.decode(string)`

JSON exposes the [CJSON](http://www.kyne.com.au/~mark/software/lua-cjson.php) class.


##### Running in environments
To run Carb in an environment different than `development` (the default), ensure this environment has been defined where appropriate (at least in `./config/settings.lua`) and run:

```bash
$ CARB_ENV={environment} carb start
```

For instance, to run Carb in production:

```bash
$ CARB_ENV=production carb start
```

To stop:

```bash
$ CARB_ENV={environment} carb stop
```
