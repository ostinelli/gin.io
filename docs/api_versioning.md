---
layout: docs
title: RALIS.IO | API Versioning
---


# API Versioning

Ralis uses the HTTP header `Accept` to version your API. The header has the format:

```
application/vnd.{application-name}.v{version}+json
```

So for instance, if your application name (as defined in the file `./config/application.lua`) is `demo`, a client trying to access the version `1` of your API will have to send the header:

```
Accept: application/vnd.demo.v1+json
```

> Ralis only accepts requests that provide JSON in the body, hence the `+json` portion of the `Accept` header.

Major versioning is baked into Ralis. You only need to define your version [routes](/docs/routes.html), and ensure that the controllers that respond to them are inside the correspondant directory `./app/controllers/{major-version}`. For example, all controllers responding to version `1` need to be placed in the `./app/controllers/1` directory.


If you are providing support for minor versioning of your APIs, a client can specify a minor version like `1.0.13-beta` by providing the header:

```
Accept: application/vnd.demo.v1.0.13-beta+json
```
See [controllers](/docs/controllers.html) on how to support minor API versioning.


##### API Console

Please note that to provide developers with an easy way to play with their API, Ralis comes with a handy [API Console](/docs/api_console.html) that takes care of API Versioning in a browser.