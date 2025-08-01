---
title: Request
weight: 8
---

This details the request object, generally the first argument
of the functions passed to paths (`app$get("/", \(req, res){})`).

## Object

Easiest to see what is available is to `print` the object.

```r
library(ambiorix)

app <- Ambiorix$new()

app$get("/", \(req, res){
  print(req)
  res$send("Using {ambiorix}!")
})

app$start()
```

```
✔ GET "/"
• HEADERS: "image/avif,image/webp,*/*", "gzip, deflate", "en-US,en;q=0.5", "keep-alive",
"localhost:9345", "http://localhost:9345/", "image", "no-cors", "same-origin", and "Mozilla/5.0 (X11;
Linux x86_64; rv:97.0) Gecko/20100101 Firefox/97.0"
• HTTP_ACCEPT: "image/avif,image/webp,*/*"
• HTTP_ACCEPT_ENCODING: "gzip, deflate"
• HTTP_ACCEPT_LANGUAGE: "en-US,en;q=0.5"
• HTTP_CACHE_CONTROL:
• HTTP_CONNECTION: "keep-alive"
• HTTP_COOKIE:
• HTTP_DNT:
• HTTP_HOST: "localhost:9345"
• HTTP_SEC_FETCH_DEST: "image"
• HTTP_SEC_FETCH_MODE: "no-cors"
• HTTP_SEC_FETCH_SITE: "same-origin"
• HTTP_SEC_FETCH_USER:
• HTTP_UPGRADE_INSECURE_REQUESTS:
• HTTP_USER_AGENT: "Mozilla/5.0 (X11; Linux x86_64; rv:97.0) Gecko/20100101 Firefox/97.0"
• httpuv.version 1.6.5
• PATH_INFO: "/favicon.ico"
• QUERY_STRING: ""
• REMOTE_ADDR: "127.0.0.1"
• REMOTE_PORT: "59462"
• REQUEST_METHOD: "GET"
• SCRIPT_NAME: ""
• SERVER_NAME: "127.0.0.1"
• SERVER_PORT: "127.0.0.1"
• CONTENT_LENGTH:
• CONTENT_TYPE:
• HTTP_REFERER: "http://localhost:9345/"
• rook.version: "1.1-0"
• rook.url_scheme: "http"
```

To access the `HEADERS` for instance, simple do `req$HEADERS`.

## Bind

{{% callout note %}}

In the very early days of ambiorix, the request was a locked environment so
one could only read from it.
Now, the environment is not locked and variables can be added
to the request, e.g.: `req$x <- 1L`.

{{% /callout %}}

Make sure you do not overwrite existing data.

```r
app <- Ambiorix$new()

app$use(\(req, res){
  # set
  req$user <- "John"
})

app$get("/", \(req, res){
  # get
  print(req$user)
  res$send("Hello {ambiorix}")
})

app$start()
```

**Counter**

This is an example of creating a counter; every refresh bumps the counter,
using a middleware means we count visits overall, not just to the main page.

```r
app <- Ambiorix$new()

val <- 0L

app$use(\(req, res){
  val <<- val + 1L
  req$x <- val
})

app$get("/", \(req, res){
  res$send_sprintf(
    "Count %s",
    req$x
  )
})

app$get("/add", \(req, res){
  res$send_sprintf(
    "Added one!",
  )
})

app$start()
```

## Parsers

Ambiorix provides built-in parsers to handle different types of request body data. These parsers make it easy to extract and work with data sent from forms, JSON APIs, and file uploads.

### JSON Parser

Use `parse_json()` to parse JSON data from request bodies:

```r
library(ambiorix)

app <- Ambiorix$new()

app$post("/api/data", \(req, res) {
  data <- parse_json(req)
  print(data)
  res$json(list(received = data))
})

app$start()
```

### Form URL-Encoded Parser

Use `parse_form_urlencoded()` to parse standard HTML form data:

```r
app$post("/form", \(req, res) {
  form_data <- parse_form_urlencoded(req)
  print(form_data$name) # Access form field by name
  res$send("Form received!")
})
```

### Multipart Form Data Parser

Use `parse_multipart()` to handle multipart form data, including file uploads:

```r
app$post("/upload", \(req, res) {
  data <- parse_multipart(req)

  # Handle regular form fields
  print(data$username)

  # Handle file uploads
  if ("file" %in% names(data)) {
    file_info <- data$file
    # file_info contains:
    # - value: Raw vector of file contents
    # - content_type: MIME type (e.g., "image/png")
    # - filename: Original filename
    # - name: Form field name

    # Save uploaded file
    temp_path <- tempfile()
    writeBin(file_info$value, temp_path)
  }

  res$send("Upload processed!")
})
```

### Custom Parsers

You can override the default parsers by setting global options:

```r
# Custom JSON parser using jsonlite
my_json_parser <- function(body, ...) {
  txt <- rawToChar(body)
  jsonlite::fromJSON(txt, ...)
}
options(AMBIORIX_JSON_PARSER = my_json_parser)

# Custom multipart parser
options(AMBIORIX_MULTIPART_FORM_DATA_PARSER = my_multipart_parser)

# Custom form URL-encoded parser
options(AMBIORIX_FORM_URLENCODED_PARSER = my_form_parser)
```

Custom parser functions must accept:

- `body`: Raw vector containing the request data
- `content_type`: Content-Type header (for multipart parser only)
- `...`: Additional optional parameters
