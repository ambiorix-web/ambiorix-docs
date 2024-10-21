---
title: Logger
weight: 17
---

Ambiorix' logger is built with [log](https://log.opifex.org).
The original `Logger` class found in early versions of ambiorix
has been deprecated in favour of the `Logger` class from
the [log](https://log.opifex.org) package.

## Auto

You can switch on the logging either with the `ambiorix.logger` option or when instantiating the application, this is now the default.

```r
library(ambiorix)

app <- Ambiorix$new(log = TRUE)

app$get("/", \(req, res){
  res$send("hello!")
})

app$get("/about", \(req, res){
  res$send("Me me me")
})

app$start()
```

Visiting both routes gives the following log.

```
✔ 27-02-2022 20:12:08 Listening on http://localhost:2488
ℹ 27-02-2022 20:12:08 GET on /
✖ 27-02-2022 20:12:09 GET on /favicon.ico - Not found
✖ 27-02-2022 20:12:38 Server stopped
```

## Druid

There is a middleware to easily implement more extensive logging,
see [druid](https://github.com/ambiorix-web/druid).

```r
library(druid)
library(ambiorix)

app <- Ambiorix$new(log = FALSE)
app$use(
  druid(
    path_info = TRUE,
    remote_port = TRUE 
  )
)

app$get("/", \(req, res){
  res$send("hello!")
})

app$get("/about", \(req, res){
  res$send("Me me me")
})

app$start()
```

```
> 27-02-2022 20:15:24 PATH_INFO: "/" REMOTE_PORT: "51970"
> 27-02-2022 20:15:24 PATH_INFO: "/favicon.ico" REMOTE_PORT: "51970"
```

## Manual

The logger used internally (above) is exported and can be used by developers: this will work regardless of whether the internal logger is on or off. Note that it will automatically prepend every event logged with the date and time at which it happened.

```r
library(ambiorix)

app <- Ambiorix$new(log = FALSE)

# create logger
logger <- new_log()

app$get("/", \(req, res){
  logger$log("Home", "was visited")
  res$send("hello!")
})

app$get("/about", \(req, res){
  logger$log("About page", "was just viewed")
  res$send("Me me me")
})

app$start()
```

Visiting both routes gives the following log.

```
> 2020-09-20 13:48:34 - Home was visited
> 2020-09-20 13:48:41 - About page was just viewed
```

## Log package

The logger used internally (above) is exported and can be used by developers: this will work regardless of whether the internal logger is on or off. Note that it will automatically prepend every event logged with the time at which it happened.

```r
library(log)
library(ambiorix)

app <- Ambiorix$new()

# create logger with the log package
logger <- Logger$new()

app$get("/", \(req, res){
  log$log("Home", "was visited")
  res$send("hello!")
})

app$get("/about", \(req, res){
  log$log("About page", "was just viewed")
  res$send("Me me me")
})

app$start()
```

Visiting both routes gives the following log.

```
> Home was visited
> About page was just viewed
```
