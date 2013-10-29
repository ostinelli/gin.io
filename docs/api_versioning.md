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

Major versioning is baked into Ralis. You only need to define your version [routes](/docs/routes.html), and ensure that the controllers that respond to them are inside the correspondant directory `./app/controllers/{major-version}`. For example, all controllers responding to version `1` need to be placed in the `./app/controllers/1` directory.


If you are providing support for minor versioning of your APIs, a client can specify a minor version like `1.0.13-beta` by providing the header:

```
Accept: application/vnd.demo.v1.0.13-beta+json
```
See [controllers](/docs/controllers.html) on how to support minor API versioning.


##### Common Error responses

If no `Accept` header is provided by a client, Ralis will respond with a `412` HTTP status and a JSON body specifying the following error:

```javascript
{
    "code": 100,
    "message": "Accept header not set."
}
```

If an invalid `Accept` header is provided by a client, Ralis will respond with a `412` HTTP status and a JSON body specifying the following error:

```javascript
{
    "code": 101,
    "message": "Invalid Accept header format."
}
```

If the API version specified by the client is not supported by your application, Ralis will respond with a `412` HTTP status and a JSON body specifying the following error:

```javascript
{
    "code": 102,
    "message": "Unsupported version specified in the Accept header."
}
```


##### API Console

Please note that to provide developers with an easy way to play with their API, Ralis comes with a handy [API Console](/docs/api_console.html) that takes care of API Versioning in a browser.