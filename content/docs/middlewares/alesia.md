---
title: Alesia
---

HTML minifier middleware for [ambiorix](https://ambiorix.dev).

## Installation

``` r
# install.packages("remotes")
remotes::install_github("ambiorix-web/alesia")
```

## Example

Simply add the middleware, it will minify the output of the
`send_file` and `render` functions, see man page `?alesia` for options.

``` r
library(alesia)
library(ambiorix)

app <- Ambiorix$new()

app$use(alesia())

app$get("/", \(req, res){
  res$render(
    "home",
    data = list(
      string = "Hello world!"
    )
  )
})

app$start()
```
