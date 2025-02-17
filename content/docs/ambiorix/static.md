---
title: Static
weight: 16
---

This section details how to serve static files, such as
images, CSS & JavaScript, using Ambiorix.

The underlying logic is inherited from [httpuv](https://github.com/rstudio/httpuv) which is also
used by Shiny, therefore this is very similar to Shiny's
`addResourcePath`.

## Serving a Static Directory

To serve static files, use the `static()` method of Ambiorix,
which takes these parameters:

- `path`: The path to the directory containing the static files.
- `uri`: The URL path where these files will be accessible
by the client.

For example, the following code:

```r
app$static("public", "www")
```

Makes the files inside the `public/` folder available under `/www`.

If there's an image named `logo.png` inside `public/`, it will
be accessible at `/www/logo.png`.

Here's a full example of how your app might look like:

```r
library(ambiorix)

app <- Ambiorix$new()

app$static(path = "path/to/static/assets", uri = "assets")

app$get("/", \(req, res){
  res$send(
    "<h1>Hello everyone!</h1>
    <img src='assets/image.png' />"
  )
})

app$start()
```

{{% callout warning%}}

All files in the directory are publicly accessible. **Do not put any sensitive files
in a static directory.**

{{% /callout %}}

## Serving Multiple Static Directories

If you need to serve multiple directories as static content,
simply call `static()` multiple times with different `path` &
`uri` values.

Consider this file structure:

```
.
├── another
│   └── the-ring.jpeg
├── index.R
└── public
    └── gollum.jpeg
```

To server both `another/` and `public/` as static directories:

```r
Ambiorix$new()$
  static("public", "assets")$
  static("another", "assets2")$
  ...
```

Here's a full example:

```r
library(ambiorix)
library(htmltools)

home_get <- \(req, res){
  html <- tagList(
    tags$h3("One ring to rule them all"),
    tags$img(src = "/assets2/the-ring.jpeg", alt = "the one ring")
  )

  res$send(html)
}

about_get <- \(req, res) {
  html <- tagList(
    tags$h3("My precious!"),
    tags$img(src = "assets/gollum.jpeg")
  )

  res$send(html)
}

Ambiorix$new()$
  static("public", "assets")$
  static("another", "assets2")$
  get("/", home_get)$
  get("/about", about_get)$
  start()
```

Now:

- `/assets2/the-ring.jpeg` serves `the-ring.jpeg` from `another/`.
- `/assets/gollum.jpeg` serves `gollum.jpeg` from `public/`.
