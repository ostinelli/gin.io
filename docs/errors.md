---
layout: docs
title: RALIS.IO | Errors
---


# Errors

As part of your application's default initializers, a `./config/initializers/errors.lua` file has been generated for you.

This is what a simple error file looks like:

```lua
Errors = {
    [1000] = { status = 400, message = "Invalid request.", headers = { ["X-Header"] = "header" } },
}
```

The global variable `Errors` contains the entries for all possible errors of your application. Every item in this table defines
an error, where:

 * The `1000` key is the error number
 * `status`  (required): is the HTTP status code
 * `message` (required): is the error description
 * `headers` (optional): are the additional headers to be returned in the response

From a controller, raising these errors is as simple as calling:

```lua
self:raise_error(1000)
```

As per this example, this will return a HTTP status `400`, with the specified headers and the JSON body:

```javascript
{
    "code": 1000,
    "message": "Invalid request."
}
```

> Your application's error numbers must be greater than `1000`. All errors numbers from `0` to `999` are reserved for Ralis.

##### Ralis error responses


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

If the JSON body provided in a request is invalid, Ralis will respond with a `400` HTTP status and a JSON body specifying the following error:

```javascript
{
    "code": 103,
    "message": "Could not parse JSON in body."
}
```

If the JSON body provided in a request is not a JSON hash, Ralis will respond with a `400` HTTP status and a JSON body specifying the following error:

```javascript
{
    "code": 104,
    "message": "Body should be a JSON hash."
}
```
