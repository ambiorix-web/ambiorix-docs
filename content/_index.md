---
title: 'Home'
date: 2024-10-17
type: landing

design:
  # Default section spacing
  spacing: "6rem"

sections:
  - block: hero
    content:
      title: Ambiorix
      text: Unopinionated, minimalist web framework for R inspired by express.js
      primary_action:
        text: Watch Video
        url: "https://www.youtube.com/watch?v=owpbIQ-j6Kk"
        icon: rocket-launch
      secondary_action:
        text: Read the docs
        url: /docs/
      announcement:
        text: "Announcing the release of version 2."
        link:
          text: "Read more"
          url: "/blog/"
    design:
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
      # For full-screen, add `min-h-screen` below
      css_class: ""
      background:
        color: ""
        image:
          # Add your image background to `assets/media/`.
          filename: ""
          filters:
            brightness: 0.5
  - block: features
    content:
      title: Full stack, right in R
      text: Easily build web applications & APIs, all in one syntax and right in R.
      items:
        - name: "Easy to use"
          description: Create applications with the tried and tested API of express.js
        - name: "One syntax"
          description: Use a single syntax to build RESTful APIs and web applications
        - name: "Extendable"
          description: Leverage existing middlewares, parsers & serializers or create your own
    design:
      # Section background color (CSS class)
      css_class: "bg-gray-100 dark:bg-gray-800"
      # Reduce spacing
      spacing:
        padding: ["1rem", 0, "1rem", 0]
  - block: features
    id: features
    content:
      title: Features
      text: Here are some of the features that make Ambiorix well-suited for building large, scalable applications
      items:
        - name: Routing
          # icon: magnifying-glass
          icon: code-bracket
          description: Build multipage applications right out of the box.
        - name: Templating
          icon: rectangle-group
          description: For Server-Side Rendering (SSR). HTML, markdown, pug, etc.
        - name: Middleware
          icon: cog
          description: Easily pre-process requests to the server.
        - name: Websockets
          icon: arrows-right-left
          description: For when you need bi-directional communication between the server & client.
        - name: Async
          icon: square-3-stack-3d
          description: Use asynchronous programming techniques by returning promises from request handlers.
        - name: Highly Customizable
          icon: bolt
          description: You have absolute full control over the request-response cycle!
  - block: cta-card
    content:
      title: "Start Writing with the #1 Effortless Documentation Platform"
      text: Hugo Blox Docs Theme brings all your technical knowledge together in a single, centralized knowledge base. Easily search and edit it with the tools you use every day!
      button:
        text: Get Started
        url: https://hugoblox.com/templates/details/docs/
    design:
      card:
        # Card background color (CSS class)
        css_class: "bg-primary-700"
        css_style: ""
---
