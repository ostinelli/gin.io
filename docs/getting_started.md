---
layout: docs
title: RALIS.IO | Getting Started
---


# Getting Started

Ready to start your first Ralis app? First, move to a workspace directory:

```bash
$ cd ~/workspace
```

Create a new Ralis app called `demo`:

```bash
$ ralis new demo
Creating app demo...
  created file demo/priv/test-server.key
  created file demo/priv/development-server.key
  created file demo/app/models/.gitkeep
  created file demo/priv/test-server.crt
  created file demo/config/settings.lua
  created file demo/priv/development-server.crt
  created file demo/spec/spec_helper.lua
  created file demo/spec/models/.gitkeep
  created file demo/config/nginx.conf
  created file demo/spec/controllers/1/pages_controller_spec.lua
  created file demo/lib/.gitkeep
  created file demo/config/application.lua
  created file demo/app/controllers/1/pages_controller.lua
  created file demo/config/database.lua
  created file demo/config/initializers/errors.lua
  created file demo/config/routes.lua
```

A new `hello world` app has been created for you. CD into the `demo` directory:

```bash
$ cd demo
```

The greenfield application just created for you comes with a simple controller and its test. You can run the test with
[Busted](http://olivinelabs.com/busted/), the test library that is already included in Ralis. To run all the tests for your application:

```bash
$ busted

1 success / 0 failures / 0 pending : 0.039959 seconds.
```

We can now start up our `demo` application. Let's do so:

```bash
$ ralis start
Ralis app in development was succesfully started on port 7200.
```

Open up the browser and point it to [https://localhost:7200/](https://localhost:7200/). You should see an error message:

```javascript
{
    code: 101,
    message: "Invalid Accept header format."
}
```
Do not worry about the fact that you're seeing this error. It is expected behavior: Ralis uses the HTTP header `Accept` to implement API versioning, therefore by accessing your Ralis `demo` application directly from your browser you're not setting it correctly, which is what Ralis is complaining about.

It is however very practical for developers to play around with their API in a browser. To support this, Ralis comes with an [API console](/docs/api_console.html) that can be accessed in a browser at the address [https://localhost:7200/ralisconsole](https://localhost:7200/ralisconsole).

Try it out now: open up the API console, then click on the `HIT` button. You should now see the Ralis `hello world` of your `demo` application, in the response body:

```javascript
{
  "message": "Hello world from Ralis!"
}
```
