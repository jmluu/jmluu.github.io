---
title: ""
date: 2026-03-31
type: landing
design:
  spacing: "6rem"

sections:
  - block: resume-biography-3
    content:
      username: me
      text: ''
      button:
        text: Download CV
        url: uploads/resume.pdf
    design:
      background:
        gradient_mesh:
          enable: true
      name:
        size: md
      avatar:
        size: medium
        shape: circle

  - block: collection
    id: papers
    content:
      title: Recent Publications
      filters:
        folders:
          - publications
    design:
      view: citation

  - block: collection
    id: projects
    content:
      title: Projects
      filters:
        folders:
          - projects
    design:
      view: article-grid
      columns: 2

  - block: collection
    id: talks
    content:
      title: Recent Events & Talks
      filters:
        folders:
          - events
    design:
      view: card

  - block: collection
    id: posts
    content:
      title: Recent Posts
      filters:
        folders:
          - blog
    design:
      view: card
---
