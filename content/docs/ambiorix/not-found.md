---
title: Not Found
weight: 12
---

You can set the 404 page in two ways, the function is identical and follows the same logic as that passed to the `get` or `post` methods.

```r
# these are equivalent
app$not_found <- \(req, res){
  res$status <- 404L
  res$send(htmltools::h2("404"))
}

app$set_404(\(req, res){
  res$status <- 404L
  res$send(htmltools::h2("Not found"))
})
```
