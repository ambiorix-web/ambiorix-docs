---
title: Projects
weight: 22
---

The easiest way to get setup is by creating an ambiorix project. This will setup a static directory, 404 page, websockets, etc.

## Package

Create the project with the
[ambiorix.generator](https://github.com/ambiorix-web/ambiorix.generator)
or with the [ambiorix-cli](https://github.com/ambiorix-web/ambiorix-cli).

### CLI

```bash
ambiorix-cli create-package myapp
```

### R

```r
ambiorix.generator::create_package("myapp")
```

_There are a number of other templates to start from._

This creates a directory with the following file structure.

```
.
├── DESCRIPTION
├── NAMESPACE
├── R
│   ├── about.R
│   ├── assets.R
│   ├── build.R
│   ├── contact.R
│   ├── docs.R
│   ├── error.R
│   └── home.R
├── app.R
└── inst
    ├── assets
    │   ├── ambiorix.js
    │   └── style.css
    └── templates
        ├── 404.html
        ├── contact.html
        ├── home.html
        └── partials
            └── header.html
```
