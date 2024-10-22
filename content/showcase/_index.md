---
title: Showcase
description: "Docs websites powered by Hugo Blox."
type: landing

sections:
  - block: hero
    content:
      title: Get Inspired
      text: 'Get inspired by exploring sites made with Ambiorix'
      primary_action:
        icon: brands/x
        text: Follow us
        url: "https://x.com/ambiorixweb"
      secondary_action:
        text: Explore More On Our GitHub Account
        url: https://github.com/ambiorix-web
    design:
      no_padding: true
      spacing:
        padding: [0, 0, 0, 0]
        margin: [0, 0, 0, 0]
  - block: collection
    content:
      filters:
        folders:
          - showcase
      sort_by: Weight
      sort_ascending: true
    design:
      view: card
      spacing:
        padding: ['3rem', 0, '6rem', 0]
---
