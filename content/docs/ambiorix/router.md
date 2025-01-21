---
title: Router
weight: 10
---

## Unnested

In order to better structure the app ambiorix comes with the ability to create _routers_. These allow having a base path prepended to every route subsequently added to it; thereby enabling to physically and mentally better structure the routing logic of an application.

Consider the application below which does not make use of a router.

```r
library(ambiorix)

# core app
app <- Ambiorix$new()

# homepage
app$get("/", \(req, res){
  res$send("Home!")
})

# /users logic
app$get("/users", \(req, res){
  res$send("List of users")
})

app$get("/users/:id", \(req, res){
  cat("Return user id:", req$params$id, "\n")
  res$send(req$params$id)
})

app$get("/users/:id/profile", \(req, res){
  msg <- sprintf("This is the profile of user #%s", req$params$id)
  res$send(msg)
})

app$start()
```

Ideally the `/users` logic should be separated from the main app, below we use the router in a `router.R` file where we place the `/users` logic. A base path is passed to the router instantiation; this will make it such that every subsequent route attached to the router will be prepended by this base path.

```r
# router.R
# create router
router <- Router$new("/users")

router$get("/", \(req, res){
  res$send("List of users")
})

router$get("/:id", \(req, res){
  cat("Return user id:", req$params$id, "\n")
  res$send(req$params$id)
})

router$get("/:id/profile", \(req, res){
  msg <- sprintf("This is the profile of user #%s", req$params$id)
  res$send(msg)
})
```

We can then simplify `app.R`: it needs to source the router from `router.R`, the router then needs to be mounted on the core application with the `use` method.

```r
library(ambiorix)

# core app
app <- Ambiorix$new()

app$get("/", \(req, res){
  res$send("Home!")
})

# mount the router
app$use(router)

app$start()
```

## Nested

To improve logical organization and reuse of middleware across your app, you sometimes need nested routers.

Nesting of routers is as easy as `router1$use(router2)`. Here's an example:

```r
library(ambiorix)
library(htmltools)

# routers + handlers:
first_router <- Router$new("/first")
first_router$get("/", \(req, res) {
  res$send(h1("Users"))
})

second_router <- Router$new("/second")
second_router$get("/", \(req, res) {
  res$send(h1("Second!"))
})

third_router <- Router$new("/third")
third_router$get("/", \(req, res) {
  res$send(h1("Third!"))
})

# middleware
second_middleware <- \(req, res) {
  cat("Hello from /second...\n")
}

# nesting:
second_router$use(second_middleware)
second_router$use(third_router)
first_router$use(second_router)

# app instance:
app <- Ambiorix$new()

app$use(first_router)

app$get("/", \(req, res) {
  page <- div(
    tags$a(href = "/first", "1"),
    tags$a(href = "/first/second", "2"),
    tags$a(href = "/first/second/third", "3")
  )
  res$send(page)
})

app$start()
```

Once you run the app, you can visit these endpoints:

- `/`
- `/first`
- `/first/second`
- `/first/second/third`

Note how the `second_middleware` runs on both `/first/second` & `/first/second/third`.
