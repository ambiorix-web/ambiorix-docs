---
title: Jader
---

Use the [pug](https://github.com/pugjs/pug)
templating engine with [ambiorix](https://ambiorix.dev).

## Installation

``` r
# install.packages("remotes")
remotes::install_github("ambiorix-web/pugger")
```

## Example

Render the following `test.pug` file.

```pug
!!! 5
html(lang="en")
  head
    title= pageTitle
    :javascript
      | if (foo) {
      |    bar()
      | }
  body
    h1 Jade - node template engine
    #container
      - if (iUseJader)
         Vape juice are amazing
      - else
         Get on it!
         Get on it!
         Get on it!
         Get on it!
```

With the following app.

``` r
library(ambiorix)

app <- Ambiorix$new()

app$engine(jader::jader())

app$get("/", \(req, res) {
  res$render(
    "test.jade",
    list(
      iUseJader = TRUE
    )
  )
})

app$start()
```

__Outside of Ambiorix__

Just use `renderer`.

```r
pugger::renderer(
  "test.pug",
  list(de
    iUsePugger = TRUE
  )
)
```
