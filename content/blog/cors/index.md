---
title: CORS
summary: How to allow Cross-Origin Resource Sharing in Ambiorix using a middleware
date: 2024-08-24
authors:
  - mwavu
tags:
  - CORS
  - Middleware
image:
  caption: 'CORS'
share: false
---

TL;DR: See the [middleware](#middleware).

## Quoting [MDN web docs](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)

Cross-Origin Resource Sharing (CORS) is an HTTP-header based mechanism that allows a server to indicate any origins (domain, scheme, or port) than its own from which a browser should permit loading resources.

CORS also relies on a mechanism by which browsers make a “preflight” request to the server hosting the cross-origin resource, in order to check that the server will permit the actual request. In that preflight, the browser sends the headers that indicate the HTTP method and headers that will be used in the actual request.

An example of cross-origin request: the front-end JavaScript code served from `https://domain-a.com` uses `fetch()` to make a request for `https://domain-b.com/data.json`.

For security reasons, browsers restrict cross-origin HTTP requests initiated from scripts.

For example, `fetch()` and `XMLHtttpRequest` follow the ‘same-origin policy’. This means that a web application using those APIs can only request resources from the same origin the application was loaded from unless the response from the other origins includes the right CORS headers.

## How to allow CORS

It's really simple. This image from [html5rocks.com](https://www.html5rocks.com/static/images/cors_server_flowchart.png) is all you need:

![CORS flowchart](cors_server_flowchart.png)

## Middleware

```r
#' Allow CORS
#'
#' @details
#' Sets these headers in the response:
#' - `Access-Control-Allow-Methods`
#' - `Access-Control-Allow-Headers`
#' - `Access-Control-Allow-Origin`
#' @export
cors <- \(req, res) {
  res$header("Access-Control-Allow-Origin", "http://127.0.0.1:8000")

  if (req$REQUEST_METHOD == "OPTIONS") {
    res$header("Access-Control-Allow-Methods", "GET, POST, PUT, DELETE")
    res$header(
      "Access-Control-Allow-Headers",
      req$HEADERS$`access-control-request-headers`
    )

    return(
      res$set_status(200L)$send("")
    )
  }
}
```

Change the values set for the headers to suit your specific use-case.

If your API requires the use of cookies or authentication tokens across domains, you will also need to set the “Access-Control-Allow-Credentials” header inside the `if () {}` block:

```r
res$header("Access-Control-Allow-Credentials", "true")
```

{{% callout note %}}

- DO NOT include a trailing slash in the allowed origins.
  - `http://127.0.0.1:8000/`❌
  - `http://127.0.0.1:8000`✅
- In Ambiorix, middlewares are executed in the order they're registered with
`use()`. Make sure this middleware is the first one in the sequence.

{{% /callout %}}
