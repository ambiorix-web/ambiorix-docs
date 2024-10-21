---
title: Middlewares
weight: 3
sidebar:
  open: false
---

Middlewares a functions that run __before__ anything in the application.

For instance the one below is simply used to print the current time,
it will run whatever the route.

```r
library(ambiorix)

app <- Ambiorix$new(log = FALSE)

# middleware
app$use(\(req, res){
  print(Sys.time())
})

app$get("/", \(req, res){
  res$send("Using {ambiorix}!")
})

app$get("/about", \(req, res){
  res$text("About")
})

app$start()
```

## Next

{{< cards >}}
  {{< card url="/docs/middlewares/agris" title="Agris" icon="document-duplicate" >}}
{{< /cards >}}
