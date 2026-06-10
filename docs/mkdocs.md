---
title: MkDocs/Zensical
---

[MkDocs](https://www.mkdocs.org/) and [Zensical](https://zensical.org/) are two engines for building documentation pages. Zensical essentially being a Rust clone of MkDocs, developed by the same people that also gave us [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/).

If you want to generate good looking static HTML from your Markdown documents without too much fuss, MkDocs is your safest bet. Citation support is still lacking from Zensical (see [Zensical issue #79](https://github.com/zensical/backlog/issues/79)).

The easiest way to use either MkDocs or Zensical is by using the `basic` style in Entangled (i.e. using Quarto attributes). If you want to use the `default` style, you may also want to use the [`mkdocs-entangled` plugin](https://github.com/entangled/mkdocs-plugin).

We have a nice example page at [entangled/mkdocs-example](https://entangled.github.io/mkdocs-example).
