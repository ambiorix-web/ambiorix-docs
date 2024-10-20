---
title: Middleware
weight: 2
---

You can also employ middleware with `use`: these are run first at every request. Note that unlike other methods (e.g.: `get`) those may return a response but do not have to.

## Existing Middlewares

List of existing middleswares. See the [documentation](/docs/middlewares)
for more details.

- [druid](https://github.com/ambiorix-web/druid) Logger
- [alesia](https://github.com/ambiorix-web/alesia) Minifier
- [eburones](https://github.com/ambiorix-web/eburones) Sessions
- [agris](https://github.com/ambiorix-web/druid) Security
- [scilis](https://github.com/ambiorix-web/scilis) Cookies
- [titan](https://github.com/ambiorix-web/titan) Prometheus middleware
- [surf](https://github.com/ambiorix-web/surf) CSRF protection
- [signaculum](https://github.com/ambiorix-web/signaculum) favicon
- [pugger](https://github.com/ambiorix-web/pugger) Pug engine
- [jader](https://github.com/ambiorix-web/jader) Jade engine
- [slighe](https://github.com/ambiorix-web/slighe) Pattern matching

_Feel free to make a PR to add to the list._

## Creating middlewares

Below we add a middleware that simply print the time at which the request is recevied.

```r
library(ambiorix)

app <- Ambiorix$new()

app$use(\(req, res){
  print(Sys.time())
})

app$get("/", \(req, res){
  res$send("Using {ambiorix}!")
})

app$get("/about", \(req, res){
  res$text("About")
})

app$start()
```

Multiple middleware can also be used. These can be used to modify add parameters to the request.

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
package of write reusable middleware.

```r
middleware <- \(prefix){
  \(req, res){
    cat(PREFIX, "-log\n")
  }
}

app$use(middleware("PREFIX"))
```
