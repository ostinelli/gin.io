---
layout: docs
title: RALIS.IO | Controllers
---


# Controllers

After the routing has been determined (see [routes](/docs/routes.html)), a controller action will get called to process the request and provide a response.

Here's what a simple controller looks like:

```lua
local InfoController = {}

function InfoController:whoami()
    return 200, { name = 'ralis' }
end

return InfoController
```

##### Return values

The return values of a controller define its response. There are three way you can do so.

###### Just the HTTP code

```lua
return 200
```

Returning only the HTTP code will respond with the provided code, an empty JSON body and no additional headers.

###### The HTTP code and the body

```lua
return 200, { name = 'ralis' }
```

Ralis will respond with the provided code and the encoded JSON body, with no additional headers.

###### The HTTP code, the body and the headers

```lua
return 200, { name = 'ralis' }, { ["Cache-Control"] = "max-age=3600", ["Retry-After"] = "120" }
```

Ralis will respond with the provided code, the encoded JSON body, and the additional headers.


##### Raising errors

Once you have defined your [errors](/docs/errors.html), it's really easy to return error responses.

To raise an error, simply refer to the error number you've defined in your errors' initializer. For instance, if your Errors are defined as:

```lua
Errors = {
    [1000] = { status = 400, message = "Invalid request." }
}
```

Then from a controller you can simply raise it with:

```lua
self:raise_error(1000)

```

This will return a HTTP status `400` with the body:

```javascript
{
    "code": 1000,
    "message": "Invalid request."
}

```

If you need to provide additional information in the payload of the error body, you can do so by passing a table as a second parameter, like so:

```lua
self:raise_error(1000, { missing_fields = { "first_name", "last_name" } })

```

This will merge the provided table and return a HTTP status `400` with the body:

```javascript
{
    "code": 1000,
    "message": "Invalid request.",
    "missing_fields": [
        "first_name",
        "last_name"
    ]
}

```

##### The routes parameters

The parameters resulting from a route match are available in the table:

```lua
self.params
```

Please see [routes](/docs/routes.html) for additional information on how to capture and use these.

##### The request

The original request is available in a controller's action and accessible on self:

```lua
self.request
```

Please note that Ralis only accepts requests that provide JSON in the body, hence the body is available to you pre-parsed for your convenience.

The request has the following properties:

 * `request.uri_params`: the parsed querystring params (a table)
 * `request.method`: the HTTP method
 * `request.headers`: the HTTP headers (a table)
 * `request.body`: the parsed JSON body (a table)
 * `request.api_version`: the complete API version as specified by the client.

It also provide these additional advanced properties:

 * `request.uri`: the full URI
 * `request.body_raw`: the raw body
 * `request.ngx`: the ngx object, refer to the [HttpLuaModule](http://wiki.nginx.org/HttpLuaModule) documentation for more information.



