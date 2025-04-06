---
title: Response
weight: 7
---

Every route (`get`, `post`, etc.) handler should accept the request (`req`) and the response (`res`). Note that routes may optionally accept a different handler for errors.

To learn more about any of the response methods described below, please see `?ambiorix::Response`.

## HTML

Send plain HTML with `send`.

```r
app$get("/html", \(req, res) {
  res$send("<h3>hello, world!</h3>")
})
```

### htmltools

It's often more convenient to use [{htmltools}](https://rstudio.github.io/htmltools/)
because it:

1. Allows you to write HTML as structured R code.
1. Makes your code more readable when generating complex HTML dynamically.

```r
library(htmltools)

app$get("/", \(req, res){
  res$send(tags$h3("hello, world!"))
})

app$get("/about", \(req, res) {
  html <- tagList(
    tags$h3("About Us"),
    tags$p("The Ambiorix R Web Framework")
  )

  res$send(html)
})
```

### Reprex

Here's a reprex using Bootstrap:

{{% collapse "Click to expand example" %}}

```r
library(ambiorix)
library(htmltools)

#' Generic HTML page
#'
#' @param ... Passed to the body tag of the html document.
#' @return [htmltools::tags$html]
#' @export
page <- \(...) {
  tags$html(
    lang = "en",
    tags$head(
      tags$meta(charset = "utf-8"),
      tags$meta(
        name = "viewport",
        content = "width=device-width, initial-scale=1"
      ),
      tags$title("HTML demo"),
      tags$link(
        href = "https://cdn.jsdelivr.net/npm/bootstrap@5.3.3/dist/css/bootstrap.min.css",
        rel = "stylesheet",
        integrity = "sha384-QWTKZyjpPEjISv5WaRU9OFeRpok6YctnYmDr5pNlyT2bRjXh0JMhjY6hW+ALEwIH",
        crossorigin = "anonymous"
      )
    ),
    tags$body(
      class = "bg-light",
      ...
    )
  )
}

#' Home page
#'
#' @export
home_page <- \() {
  bgs <- c("primary", "success", "secondary", "dark", "danger", "white", "light")
  bg_divs <- lapply(bgs, \(bg) {
    class <- c(
      "col-12 col-md-4",
      "border border-dark-subtle",
      paste0("bg-", bg)
    )

    tags$div(
      class = class,
      style = "min-height: 250px"
    )
  })
  bg_divs <- tags$div(
    class = "row",
    bg_divs
  )

  page(
    tags$div(
      class = "container vh-100 bg-white",
      tags$h3("Hello, World!"),
      tags$p("This example is using bootstrap 5.3.3"),
      bg_divs
    )
  )
}

#' Handler for GET at '/'
#'
#' @export
home_get <- \(req, res) {
  res$send(home_page())
}

Ambiorix$new(port = 3000L)$
  get("/", home_get)$
  start()
```

{{% /collapse %}}

## Sendf

A convenient wrapper around `sprintf` and the `send`.

```r
app$get("/text", \(req, res){
  res$sendf("Hello %s", req$user)
})
```

## Text

Send a plain text with `text`.

```r
app$get("/text", \(req, res){
  res$text("hello!")
})
```

## File

An `.html` or `.R` file can also be used as response.

```r
# sends templates/home.html
app$get("/file", \(req, res){
  res$send_file("home.html")
})
```

## Render

An `.html`, `.md` or `.R` file can also be rendered. The difference with `send_file` is that it will use `data` to process `[% tags %]`. You can read more it in the templates documentation.

```r
# renders templates/home.html
# replaces [% title %]
app$get("/:book", \(req, res){
  res$render("home.html", data = list(title = req$params$book))
})

# renders docs/index.md
app$get("/docs", \(req, res) {
  res$render("index.md")
})
```

## JSON

You can also send JSON responses with `json`, e.g.: to build an api

```r
app$get("/:book", \(req, res){
  res$json(cars)
})
```

## Status

The HTTP status of the response can be specified in two ways:

- `status` active binding
- `set_status()` method

```r
app$get("/error", \(req, res){
  res$status <- 500L
  res$send("Error!")
})

# or
app$get("/error", \(req, res){
  res$set_status(500L)$send("Error!")
})
```

## CSV

Serialises to CSV, when this endpoint is visited the CSV file is downloaded. It takes the data as first argument and the name of the file to download as second argument.

```r
app$get("/csv", \(req, res){
  res$csv(cars, "cars-data")
})
```

## TSV

Serialises to tab separated file; it takes the same arguments as the csv response.

```r
app$get("/tsv", \(req, res){
  res$tsv(mtcars, "more-cars")
})
```

## Image

To send `.png` or `.jpeg` files, use the `image()` method:

```r
app$get("/cute-cat", \(req, res) {
  res$image(file = "/path/to/local/file")
})
```

If you prefer, you can be more specific and use the `png()` & `jpeg()` methods:

```r
app$get("/cute-cat-png", \(req, res) {
  res$png(file = "/path/to/local/png/file")
})

app$get("/cute-cat-jpeg", \(req, res) {
  res$jpeg(file = "/path/to/local/jpeg/file")
})
```

## ggplot2

Send a ggplot2 plot using the `ggplot2()` method:

```r
app$get("ggplot", \(req, res) {
  # make the plot:
  p <- ggplot2::ggplot(
    data = iris,
    mapping = ggplot2::aes(
      x = Sepal.Length,
      y = Petal.Width,
      color = Species
    )
  ) +
    ggplot2::geom_point() +
    ggplot2::theme_bw()

  res$ggplot2(plot = p, type = "jpeg") # or "png"
})
```

Any further parameters given to `res$ggplot2()` are
passed to `ggplot2::gsave()` function. eg. `width` & `height`.

## htmlwidgets

Serialises an htmlwidget.

```r
library(echarts4r)

app$get("/htmlwidget", \(req, res){
  plot <- e_charts(cars, speed) %>% 
    e_scatter(dist)
  res$htmlwidget(plot)
})
```

## Headers

You can add headers with the `header` method on the response object.

```r
app$get("/hello", \(req, res){
  res$header("Content-Type", "something")
  res$send("Using {ambiorix}!")
})
```

If you have several headers, put them in a named list and use the
`set_headers()` method:

```r
app$get("/hello", \(req, res) {
  headers <- list(
    "Content-Type" = "something",
    "Access-Control-Allow-Origin" = "https://ambiorix.dev",
    "Header-Name" = "Header Value"
  )
  res$set_headers(headers)
  res$send("Using {ambiorix}")
})
```

In addition, there are several methods for setting the `Content-Type`
header of the response. These will come in handy when you've written your own
custom serializers. They all have the prefix `header_content_`:

- `res$header_content_json()`
- `res$header_content_html()`
- `res$header_content_plain()`
- `res$header_content_csv()`
- `res$header_content_tsv()`

## Cookies

To set a cookie, use the `cookie()` method:

```r
app$get("/hello", \(req, res) {
  today <- as.character(Sys.Date())
  res$cookie(name = "today", value = today)
  res$send("Hello! Cookie 'today' has been set.")
})
```

To clear a cookie, use the `clear_cookie()` method:

```r
app$get("/hello2", \(req,res) {
  res$clear_cookie(name = "today")
  res$send("Cookie 'today' cleared!")
})
```

## Redirect

One can also redirect to a different url, note that these should have a `status` starting in `3`.

```r
app$get("/redirect", \(req, res){
  res$status <- 302L
  res$redirect(path = "/")
})
```

## Hooks

Hooks are functions that run before or after rendering, allowing for
pre-processing and post-processing of content.

### Pre-render hooks

A pre-render hook runs before the `render()` and `send_file()` methods. Pre-render
hooks are meant to be used as middlewares to, if necessary, do pre-processing
before rendering.

It must accept at least 4 arguments:

- `self`: The Request class instance.
- `content`: String. [File] content of the template.
- `data`: Named list. Passed from the `render()` method.
- `ext`: String. File extension of the template file.

Include `...` in your hook to ensure it will handle potential updates
to hooks in the future.

The pre-render hook must return an object of class 'responsePreHook' as
obtained by `ambiorix::pre_hook()`.

```r
my_prh <- \(self, content, data, ext, ...) {
  data$title <- "Mansion"
  pre_hook(content, data)
}

#' Handler for GET at '/'
#' 
#' @details Renders the homepage
#' @export
home_get <- \(req, res) {
  res$pre_render_hook(my_prh)
  res$render(
    file = "page.html",
    data = list(
      title = "Home"
    )
  )
}
```

In the above example, even though we have provided the title to `render()`
as "Home", it is changed in `my_prh()` to "Mansion".

### Post-render hooks

A post-render hook runs after the rendering of HTML. It must be a function
that accepts at least 3 arguments:

- `self`: The 'Response' class instance.
- `content`: String. [File] content of the template.
- `ext`: String. File extension of the template file.

Also, include `...` in your hook to ensure it will handle potential
updates to hooks in the future.

Ideally, it should return the `content`.

```r
my_prh <- \(self, content, ext, ...) {
  print("Done rendering!")
  content
}

#' Handler for GET at '/'
#'
#' @details Renders the homepage.
#'
#' @export
home_get <- \(req, res) {
  res$
    post_render_hook(my_prh)$
    render(
    template_path("page.html"),
    list(
      title = "Home",
      content = home()
    )
  )
}
```

After each render on the home page, `my_prh()` will print "Done rendering!" on the console.

### Setting a Global Hook

You can set a global pre-render or post-render hook using middleware.

This is useful when you need to ensure that all rendering operations
automatically apply the hook without explicitly setting it in every
route.

Consider the following template, `page.html`:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <title>[% title %]</title>
</head>

<body>

  <div>
    [% content %]
  </div>

</body>

</html>
```

And the corresponding `index.R`:

```r
library(ambiorix)

#' A pre-render hook
#'
#' @param self The request class instance.
#' @param content String. [file] content of the template.
#' @param data Named list. Passed from the [render()] method.
#' @param ext String. File extension of the template file.
my_prh <- \(self, content, data, ext, ...) {
  data$title <- "Mansion"
  pre_hook(content, data)
}

#' Middleware to set a global pre-render hook
#'
#' @export
m1 <- \(req, res) {
  res$pre_render_hook(my_prh)
}

#' Handler for GET at '/'
#'
#' @details Renders the homepage
#' @export
home_get <- \(req, res) {
  res$render(
    file = "page.html",
    data = list(
      title = "Home",
      content = "<h3>hello, world</h3>"
    )
  )
}

Ambiorix$new(port = 5000L)$
  set_error(error_handler)$
  use(m1)$
  get("/", home_get)$
  start()
```

Notice how even though `res$render()` sets the title as `"Home"`,
the global pre-render hook `my_prh()`, modifies it to `"Mansion"`.

Since the middleware `m1` is applied globally, this change affects
all rendering operations across the application.
