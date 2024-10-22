---
title: Middleware
weight: 9
---

Middlewares a functions that run __before__ anything in the application. They are mostly
used to modify or add parameters to the request object.

## Creating middlewares

Similar to request handlers, middlewares take the Request and Response objects
as arguments.

```r
show_time <- \(req, res) {
  print(Sys.time())
}
```

The `use()` method employs a middleware to your app instance.

```r
app$use(show_time)
```

Here's a full reprex:

```r
library(ambiorix)

app <- Ambiorix$new(log = FALSE)

app$use(show_time)

app$get("/", \(req, res){
  res$send("Using {ambiorix}!")
})

app$get("/about", \(req, res){
  res$text("About")
})

app$start()
```

Unlike other request handlers which must return a response, middlewares may but do not have to.

Multiple middleware can also be used. They are run in the order in which they're `use`d:

```r
library(ambiorix)

app <- Ambiorix$new()

app$use(\(req, res){
  req$x <- 1 # set x to 1
})

app$use(\(req, res){
  print(req$x)
})

app$get("/", \(req, res){
  res$sendf("x set to %s", req$x)
})

app$start()
```

## Common Pattern

Existing middlewares tend to use function factories, which is useful if you want to
package reusable middleware.

```r
factory <- \(prefix){
  \(req, res){
    cat(prefix, "-log\n")
  }
}

m1 <- factory("HELLO")
m2 <- factory("WORLD")

app$use(m1)$use(m2)
```

## Existing middlewares

See some [Existing Middleware](/docs/middlewares) and how you can use them.
