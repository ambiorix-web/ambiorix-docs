---
title: From Shiny
weight: 3
---

Though both [shiny](https://github.com/rstudio/shiny) and ambiorix are built on top of the same package, [httpuv](https://github.com/rstudio/httpuv), they work very differently.

While shiny is meant to build single page applications (SPA) with heavy
bi-directional communication between the server and client via websocket,
ambiorix is certainly not as opinionated and though one can build applications
very similar to shiny applications (SPA) using websockets with ambiorix since
it allows multiple pages one is likely to make use of those instead.

{{% callout note %}}

Designing single page applications will, in all likelihood,
always be faster with Shiny.
This is because shiny takes care of much of the front-end for the developer
(pre-built inputs, Bootstrap UI, etc.), while ambiorix is unopinionated
and thus handles none of this for you.

{{% /callout %}}

## Inputs

Input data can be either `POST`ed (form) or used as is done in shiny using the
websocket to send values. "Inputs" in this context more broadly includes any
data that travels from the client to server (and optionally back again).
Note that websocket communication is always initiated by the client
(in shiny too of course).

{{% callout note %}}

You can either use the bare websocket from JavaScript or import the
small helper library for convenience.

{{% /callout %}}

Websocket in ambiorix mimic shiny's custom messages, they take a `name` (unique identifier) and the message itself: both in R and JavaScript.

```r
app$receive("hello", \(msg, ws){
  print(msg)
  ws$send("bye", "Goodbye")
})
```

Above we print the `msg` received and `send` a response.

## Outputs

Outputs, or data that travels from the server to the client can also go through the websocket or be served as an HTTP response. Note that some form of that is available in shiny (but only for a unique session) though it appears to be rarely used.

```r
app$post("/submit", \(req, res){
  body <- parse_multipart(req$body)
  res$send(h1("Your name is", body$first_name))
})
```

It will likely always be faster and easier to create applications with shiny!
