---
title: Parse raw JSON
summary: An example showing how you can write/use your own parsers
date: 2024-07-29
authors:
  - mwavu
tags:
  - JSON
  - parser
image:
  caption: "JSON"
share: false
---

---

**UPDATE:** Ambiorix now has built-in parsers. You may not need to build your
own JSON parser. See [parsers](/docs/ambiorix/request/#parsers)
for:

- `parse_json()`,
- `parse_multipart()`, and
- `parse_form_urlencoded()`

---

Sometimes you need to parse requests which have raw JSON in the body.

`ambiorix::parse_multipart()` only works for 'form-data'. But Ambiorix
does not limit you to that. You can write your own request parsers or
make use of other packages. In this case, we'll use `webutils::parse_http()`.

## Example

Say we want to select columns and filter rows
in the `iris` dataset when the request body is a JSON object like this:

```r
{
    "cols": ["Sepal.Length", "Petal.Width", "Species"],
    "species": ["virginica", "setosa"]
}
```

## Parser

Let's write the parser:

```r
box::use(
  webutils[parse_http],
)

#' Parse HTTP request
#'
#' @description
#' Parses the body of an HTTP request based on the `Content-Type` of the
#' request header.
#' @details
#' Currently supports three most common types:
#' - application/x-www-form-urlencoded
#' - multipart/form-data
#' - application/json
#' @param req Request object.
#' @return Named list. Data associated with the request.
#' @export
parse_req <- \(req) {
  parse_http(
    body = req$rook.input$read(),
    content_type = req$CONTENT_TYPE
  )
}
```

With this, you can parse requests inside your handlers like so:

```r
req_data <- parse_req(req)
```

## Reprex

Here's a full reprex:

```r
box::use(
  ambiorix[Ambiorix],
  webutils[parse_http],
)

#' Handle POST at '/'
#'
#' @param req Request object.
#' @param res Response object.
#' @return `res$json()`
#' @export
home_post <- \(req, res) {
  content_type <- req$CONTENT_TYPE
  body <- req$rook.input$read()

  if (length(body) == 0L) {
    response <- list(
      code = 400L,
      msg = "Invalid request"
    )

    return(
      res$set_status(400L)$json(response)
    )
  }

  postdata <- parse_http(body, content_type)

  # filter & select as necessary:
  row_inds <- iris$Species %in% postdata$species
  col_inds <- colnames(iris) %in% postdata$cols
  data <- iris[row_inds, col_inds, drop = FALSE]

  response <- list(
    code = 200L,
    msg = "success",
    data = data
  )

  res$json(response)
}

app <- Ambiorix$new(port = 3000, host = "127.0.0.1")

app$
  post("/", home_post)$
  start()
```
