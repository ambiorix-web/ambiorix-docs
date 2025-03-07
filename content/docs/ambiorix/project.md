---
title: Projects
weight: 22
---

If you need boilerplate code to get started, you can create an Ambiorix project.

An Ambiorix project provides a structured boilerplate,
setting up a static directory, a 404 page, websockets, and more.

You can generate a project using either the
[**ambiorix.generator**](/docs/generator) package in
R or the [**ambiorix-cli**](/docs/cli) command-line tool.

## Using the CLI

To create a new Ambiorix package using the command-line interface, run:

```bash
ambiorix-cli create-package myapp
```

## Using R

Alternatively, you can generate a project within R using:

```r
ambiorix.generator::create_package("myapp")
```

_Additional templates are available._

## Project Structure

Once created, your project will have the following structure:

```
myapp/
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

This structure makes it easier to manage assets, templates, and application logic.
