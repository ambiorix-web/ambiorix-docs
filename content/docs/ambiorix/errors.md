---
title: Errors
weight: 2
---

This details how to handle errors in ambiorix.

When an error occurs server-side it should send the client a response with a status starting in `5` to indicates that was the case.

## Default

If you created your project using the [ambiorix.generator](./06-project.md) or the [ambiorix-cli](./06-project.md), ambiorix defaults to using the following handler on error:

```r
render_500 <- \(req, res, error) {
  res$status <- 500L
  res$send("Internal server error")
}
```

Otherwise, no error handler is put in place and you have to create one.

## Global

One can specify the handler to use when an error occurs anywhere in the application.

The error handler should be a function that takes 3 arguments: `req`, `res` and the `error` object.

A basic example:

```r
app$error <- \(req, res, error){
  res$status(500L)
  res$send("There was a server error :(")
}
```

You have absolute full control in what you choose to do with the `error` object. In most cases, you will need to log errors.

Here is a more involved and realistic example:

```r
error_handler <- \(req, res, error = NULL) {
  if (!is.null(error)) {
    error_msg <- conditionMessage(error)
    cli::cli_alert_danger("Error: {error_msg}")
  }
  response <- list(
    code = 500L,
    msg = "A server error occurred!"
  )
  res$
    set_status(500L)$
    json(response)
}
```

There are 2 ways you can use the handler:

1. Via the `set_error()` method:

    ```r
    Ambiorix$
      new()$
      set_error(error_handler)$
      ...
    ```

    The advantage of this is that you can chain other operations.
1. The old way, via the `error` field:

    ```r
    app <- Ambiorix$new()
    app$error <- error_handler
    ...
    ```

## Route Specific

Alternatively one can specify errors specific to certain routes, if these are not specified the global handler (above) is used.

```r
app$get("/error", \(req, res){
  print(eRrOr)
}, \(req, res, error){
  res$send("This is an error on /error", status = 500L)
})
```
