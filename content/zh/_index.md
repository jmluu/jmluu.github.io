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
        text: 下载简历
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
      title: 近期论文
      filters:
        folders:
          - publications
    design:
      view: citation

  - block: collection
    id: projects
    content:
      title: 项目
      filters:
        folders:
          - projects
    design:
      view: article-grid
      columns: 2

  - block: collection
    id: talks
    content:
      title: 近期活动与报告
      filters:
        folders:
          - events
    design:
      view: card

  - block: collection
    id: posts
    content:
      title: 近期博客
      filters:
        folders:
          - blog
    design:
      view: card
---
