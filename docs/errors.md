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