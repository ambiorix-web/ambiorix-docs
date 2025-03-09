---
title: Templates 
weight: 16.5
---

Ambiorix supports **HTML and Markdown templates** with `res$render()`.  

Templates are referenced by their **file path** and **extension** in the
`render` method.  

-----

## HTML Templates  

You can use `.html` files as templates and insert dynamic content
using placeholders (`[% variable %]`).  

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
  <p>Welcome to Ambiorix!</p>
</body>
</html>
```

Render the template in Ambiorix:  

```r
res$render("templates/home.html", data = list(title = "Hello from R"))
```

### Example Output  

Visiting `/home` would render:  

```html
<h1 class="brand">Hello from R</h1>
<p>Welcome to Ambiorix!</p>
```

-----

## Markdown Templates  

Markdown (`.md`) templates work similarly. You can insert placeholders into
Markdown files and render them dynamically.  

```md
<!-- templates/home.md -->
# [% title %]  

A list of numbers:

- 1  
- 2  
- 3  
```

Render with:  

```r
res$render("templates/home.md", data = list(title = "Hello from R"))
```

-----

## Partials (Reusable Components)  

Partials allow reusing **common HTML snippets**, similar to components in
other frameworks.  

Use the `[! partial_name.html !]` syntax to include a partial from, say,
the `templates/partials/` directory.  

### Example  

#### Main Template (`templates/home.html`)  

```html
<!DOCTYPE html>
<html lang="en">
<head>
  [! partials/header.html !]
  <title>Ambiorix</title>
</head>
<body>
  <h1 class="brand">Hello</h1>
</body>
</html>
```

#### Partial (`templates/partials/header.html`)  

```html
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<link rel="stylesheet" href="/static/style.css">
<script src="/static/ambiorix.js"></script>
```

#### Rendered Output  

```html
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
