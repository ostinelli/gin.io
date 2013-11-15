---
layout: docs
title: GIN.IO | Testing
---


# Testing
Testing is supported out of the box in Gin. The main testing framework is [Busted](http://olivinelabs.com/busted/), which has an [RSpec](http://rspec.info/)-like syntax. If you ever used [Jasmine](http://pivotal.github.io/jasmine/) you'll feel right at home.

In addition, Gin provides test helpers for you to use.

#### Controllers
Controller Tests in Gin are actually integration tests. An instance of OpenResty nginx will be started and a real request will be issued to a test server.

The main Gin test helper you'll need to use is the function `hit`, that will perform the integration test for you.

Here's an example of a controller test (file located in `./spec/controllers/1/info_controller_spec.lua`):

```lua
require 'spec.spec_helper'

describe("InfoController", function()
    describe("#whoami", function()
        it("responds with whoami name", function()
            local response = hit({
                method = 'GET',
                path = "/whoami"
            })

            assert.are.same(200, response.status)
            assert.are.same({ name = "gin" }, response.body)
        end)
    end)
end)
```

###### The request
The `hit` helper takes a list of options:

 * `method` (required): the HTTP method (`GET`, `POST`, `HEAD`, `OPTIONS`, `PUT`, `PATCH`, `DELETE`, `TRACE`, `CONNECT`)
 * `path` (required): the relative path
 * `body` (optional): the request body (a Lua `table`). Defaults to `nil`.
 * `headers` (optional): the request headers (a Lua `table`). Defaults to `{}`.
 * `api_version` (optional): if you need to specify a minor version of the APIs, you can pass in the complete version value here (for instance, `1.0.2-rc1`). Defaults to the controller test namespace.

###### The response
The `hit` helper returns a response object, that has the following attributes:

 * `status`: the response status (a numeric value)
 * `headers`: the response headers (a Lua `table`).
 * `body`: the response body (a Lua `table`).
 * `body_raw`: the unparsed response body (a `string`).


#### Models
There aren't specific Gin test helpers for models. Please refer to the [tutorial](/tutorial.html) for examples on how to test your models.


#### Test database fixtures
It is also possible to populate the test database with the SQL ORM. Please refer to the [tutorial](/tutorial.html) for examples on how to do it.