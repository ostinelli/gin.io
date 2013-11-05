---
layout: docs
title: ZEBRA.IO | Other
---

# Other

##### The Zebra object
In your application, `Zebra` exposes some attributes you might need.

 * `Zebra.env`: returns the current environment (`development`, `test`, `production`, or any other environment you may be running Zebra in)
 * `Zebra.version`: returns the current Zebra version
 * `Zebra.settings`: returns the global settings defined per environment in `./config/settings.lua` (see [Application files](/docs/application_files.html), under `settings.lua`)


##### JSON
If you need to manually encode/decode JSON, you can do so by using the global JSON object:

 * to encode: `JSON.encode(table)`
 * to decode: `JSON.decode(string)`

JSON exposes the [CJSON](http://www.kyne.com.au/~mark/software/lua-cjson.php) class.


##### Running in environments
To run Zebra in an environment different than `development` (the default), ensure this environment has been defined where appropriate (at least in `./config/settings.lua`) and run:

```bash
$ ZEBRA_ENV={environment} zebra start
```

For instance, to run Zebra in production:

```bash
$ ZEBRA_ENV=production zebra start
```

To stop:

```bash
$ ZEBRA_ENV={environment} zebra stop
```
