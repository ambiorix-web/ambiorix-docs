---
title: Usage
weight: 2
---

Simply call one of the `create_*` functions to setup your
project.

## Package

Creating an ambiorix project as an R package allows you to leverage all of the R toolchain out of the box, such as testing, or documenting.

```r
# ambiorix.generator::create_package("path-to-create-app-in")
# eg.
ambiorix.generator::create_package("myapp")
```

## Box

With [`{box}`](https://klmr.me/box/) you get to enjoy all the benefits of a modular & nested app structure.

This is ideal for large projects that benefit from a clear separation of
concerns and better organization.

```r
# ambiorix.generator::create_box(
#   "path-to-create-app-in",
#   "project-type"
# )
# eg.
```

```r
ambiorix.generator::create_box("myapi", "backend")
```

```r
ambiorix.generator::create_box("myapp", "frontend")
```
