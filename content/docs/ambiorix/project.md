---
title: Projects & Templates
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

## Templates

A project allows using templates and rendering them with `res$render`.

{{% callout note %}}

In older versions of ambiorix templates had to be placed in a
`templates` directory, this is no longer the case.
However, templates must be referenced by their path and with
their extension in the `render` or `send_file` methods.

{{% /callout %}}

### HTML

One can use HTML templates (`.html` files).

```html
<!-- templates/home.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="/static/style.css">
  <script src="/static/ambiorix.js"></script>
  <title>Ambiorix</title>
</head>
<body>
  <h1 class="brand">[% title %]</h1>
</body>
</html>
```

This is rendered with the same method.

```r
res$render("home.html", data = list(title = "Hello from R"))
```

## Markdown

One can use markdown templates (`.md` files).

```md
# [% title %]

A list

- 1
- 2
- 3
```

```r
res$render("home.md", data = list(title = "Hello from R"))
```

### Partials

You can also use partials (inspired by [gohugo](https://gohugo.io)), these are blocks of reusable HTML content. They are indicated by a different tag: `[! partial_name.html !]` where the `partial_name.html` points to a file in the `templates/partials` directory.

Therefore the template below (`templates/home.html`).

```html
<!-- templates/home.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  [! header.html !]
  <title>Ambiorix</title>
</head>
<body>
  <h1 class="brand">Hello</h1>
</body>
</html>
```

Imports the HTML at: `templates/partials/header.html`

```html
<!-- templates/partials/header.html -->
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="/static/style.css">
<script src="/static/ambiorix.js"></script>
```

To produce the following output.

```html
<!-- templates/home.html -->
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <link rel="stylesheet" href="/static/style.css">
  <script src="/static/ambiorix.js"></script>
  <title>Ambiorix</title>
</head>
<body>
  <h1 class="brand">Hello</h1>
</body>
</html>
```
