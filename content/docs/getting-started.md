---
title: Getting Started
date: 2024-02-17
weight: 1
---

## Quick Start

{{% steps %}}

### Install {ambiorix}

The stable version is available on CRAN:

```r
install.packages("ambiorix")
```

Alternatively, install the development version from GitHub with the
[{remotes}](https://remotes.r-lib.org/) package:

```r
remotes::install_github("ambiorix-web/ambiorix")
```

### Create a new project

Create a directory to hold your application, and make that your
working directory:

```bash
mkdir hello-world
cd hello-world
```

Create a new file called `app.R`:

```bash
touch app.R
```

### Hello, World

Put this in `app.R`:

```r
library(ambiorix)

app <- Ambiorix$new(port = 8000L)

app$get("/", \(req, res) {
  res$send("Hello, World!")
})

app$start()
```

### Run the app

`app.R` is the entrypoint. To start the app, run this on the terminal:

```bash
Rscript app.R
```

Alternatively, if you're using an IDE like Rstudio or Positron, highlight everything in `app.R` and run the code.

Visit [localhost/8000](http://localhost:8000).

{{% /steps %}}

You just built your first Ambiorix app.

🎉Congratulations!🎉

## Next

Let's take a closer look at the hello world app:

{{< cards >}}
  {{< card url="/docs/ambiorix/hello-world" title="Hello, World!" icon="document-duplicate" >}}
  {{< card url="/docs/ambiorix/routing" title="Routing" icon="adjustments-vertical" >}}
{{< /cards >}}
