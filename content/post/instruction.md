---
author: Hugo Authors
categories:
- Help
comment: true
date: "2021-07-19T10:52:59+08:00"
description: A clean and minimal Hugo theme porting from Minima on Hexo and adapted
  to gisma-courses need. [Click me](https://h.xjj.pub/) to take a look at the demo
  site of the original theme.
math: true
title: Instruction
weight: 2
---



## Features

- [x] Light/dark mode
- [x] VSCode-style code hightlighting
- [x] KaTeX
- [x] Mermaid
- [x] Google analytics
- [x] Disqus/Utterances comments
- [x] Optional ToC 

## Usage

Before using Minima, make sure you've got Hugo(v0.41.0 or higher) installed on your device.

### Installation

Suppose `blog` is where you place your sources.

```bash
cd blog
git init
git submodule add --depth 1 https://github.com/mivinci/hugo-theme-minima.git themes/minima
```

Or  `clone` directly if you don't wanna use the Git submodule.

```bash
git clone https://github.com/mivinci/hugo-theme-minima.git themes/minima
```

Anyways, you just need to put Minima under the `blog/themes` directory.

### Configuration

Follow [exampleSite/config.toml](https://github.com/Mivinci/hugo-theme-minima/blob/main/exampleSite/config.toml) and edit your own `blog/config.toml` file.

### Supported Front Matters

| Field       | Type   | Explanation                                            |
| :---------- | :----- | :----------------------------------------------------- |
| title       | string | Title of the post, will be rendered in a `h1` tag      |
| description | string | Description of the post, will be rendered in a `p` tag |
| date        | string | Datetime that the post is written                      |
| categories  | array  | Array of categories the posts belongs to               |
| tags        | array  | Array of tags the posts is related to                  |
| math        | bool   | True if the post needs to render math formulas         |
| mermaid     | bool   | True if the post needs to render mermaid graphs        |
| comment     | bool   | True if the post allows people to make comments        |
| draft       | bool   | True if the post isn't already to production           |
| toc         | bool   | True if the post needs a ToC                           |



### Feedback

Feedbacks to the original theme are welcome [here](https://github.com/Mivinci/hugo-theme-minima/issues).
Feedbacks to the adapted theme are welcome too [here](https://github.com/gisma-courses/hugo-theme-minima-gisma/issues).