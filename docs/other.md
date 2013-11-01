---
layout: docs
title: RALIS.IO | Other
---

# Other

##### The Ralis object
In your application, `Ralis` exposes some attributes you might need.

 * `Ralis.env`: returns the current environment (`development`, `test`, `production`, or any other environment you may be running Ralis in)
 * `Ralis.version`: returns the current Ralis version
 * `Ralis.settings`: returns the global settings defined per environment in `./config/settings.lua` (see [Application files](/docs/application_files.html), under `settings.lua`)


##### JSON
If you need to manually encode/decode JSON, you can do so by using the global JSON object:

 * to encode: `JSON.encode(table)`
 * to decode: `JSON.decode(table)`

JSON exposes the [CJSON](http://www.kyne.com.au/~mark/software/lua-cjson.php) class.


##### Running in environments
To run Ralis in an environment different than `development` (the default), ensure this environment has been defined where appropriate (at least in `./config/settings.lua`) and run:

```bash
$ RALIS_ENV={environment} ralis start
```

For instance, to run Ralis in production:

```bash
$ RALIS_ENV=production ralis start
```

To stop:

```bash
$ RALIS_ENV={environment} ralis stop
```
