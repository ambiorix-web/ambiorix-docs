---
title: Parameters & Query
weight: 5
---

Ambiorix allows extracting values from the URL
using **parameters** and **query strings**.  

- **Parameters** are dynamic parts of the URL,
accessible via `req$params$param_name`.  
- **Query strings** are key-value pairs appended to the
URL (`?key=value`), accessible via `req$query$query_name` or
`req$query[[index]]` (if unnamed).  

-----

## Parameters  

Define URL parameters using `:<param>`, and retrieve them with
`req$params$<name>`.  

```r
#' Handle GET at '/books/:category'
#'
#' @export
get_book_category <- \(req, res) {
  html <- tags$h3(
    "Books in category:",
    req$params$category
  )
  res$send(html)
}

app <- Ambiorix$new()
app$get("/books/:category", get_book_category)
app$start()
```

### Example Routes  

- `/books/fiction` → Displays **"Books in category: fiction"**  
- `/books/math` → Displays **"Books in category: math"**  
- `/books/philosophy` → Displays **"Books in category: philosophy"**  

-----

## Query Strings  

Query strings allow passing additional information to a request.
These values are parsed into `req$query`.  

```r
library(ambiorix)
library(htmltools)

#' Handle GET at '/greet'
#'
#' @export
say_hello <- \(req, res) {
  html <- tags$h3(
    "Hello,",
    req$query$firstname,
    req$query$lastname
  )

  res$send(html)
}

app <- Ambiorix$new()
app$get("/greet", say_hello)
app$start()
```

### Example Routes  

- `/greet?firstname=John&lastname=Coene` → Displays **"Hello, John Coene"**  
- `/greet?firstname=Alice&lastname=Smith` → Displays **"Hello, Alice Smith"**  
- `/greet?firstname=Marie&lastname=Curie` → Displays **"Hello, Marie Curie"**  
