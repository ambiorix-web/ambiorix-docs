---
title: Hello, World
weight: 1
---

## Video

Watch this short video introducing the basics of the framework:

<iframe width="560" height="315" src="https://www.youtube.com/embed/owpbIQ-j6Kk?si=FM5P0T-YFQpMr_8s" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

## Example

Say we have this example app:

```r
library(ambiorix)

# port to listen on:
PORT <- 3000

# initialize a new ambiorix instance:
app <- Ambiorix$new()

# respond with “Hello World!” for requests to the root URL (/) or route:
app$get("/", \(req, res) {
  res$send("Hello World!")
})

# you can also use html tags:
app$get("/about", \(req, res) {
  res$send("<h1>About Us</h1>")
})

# start server:
app$start(port = PORT)
```

This app starts a server and listens on port 3000 for connections.
It responds with "Hello World!" for requests to the root URL (`/`).

- Navigate to [localhost:3000/](http://localhost:3000/) on your browser to see that.
- Navigate to [localhost:3000/about](http://localhost:3000/about) to view the about page.

For every other path, it will respond with a **404 Not Found.**

## Port

By default, Ambiorix serves an application at a randomly available port.

In the example app, try not to define a port ie. only have `app$start()` instead of `app$start(port = PORT)`.

Defining a port (manually or programmatically) is crucial especially during
deployment, so that your app is correctly exposed to the outside world.

## Handlers

A request handler must be a function that takes two objects:

- request object (`req`)
- response object (`res`)

In the example, we have two handlers: one handling requests made at `/` and the
other for requests made at `/about`.

As your app grows, it will be better to assign the handlers a name and use that:

```r
library(ambiorix)

home_get <- \(req, res) {
  res$send("Hello World!")
}

about_get <- \(req, res) {
  res$send("<h1>About Us</h1>")
}

PORT <- 3000L

Ambiorix$
  new(port = PORT)$
  get("/", home_get)$
  get("/about", about_get)$
  start()
```

This program is equivalent to what we had before, but more readable.

As your handlers grow in size and number, it will be easier & better to
have them in separate files or folders. Since Ambiorix is unopinionated,
it is up to you to use whichever structure you work best with!

## Host

By default, Ambiorix uses `0.0.0.0` as the host. If need be, you can change the
host via the `new()` or `start()` methods:

```r
Ambiorix$
  new(port = PORT, host = "127.0.0.1")$
  ...
```

```r
Ambiorix$
  new()$
  ...
  start(port = PORT, host = "127.0.0.1")
```

- `0.0.0.0`: Tells the server to listen on every available network interface.
- `127.0.0.1`: Instructs the server to only listen for connections on the same host.
