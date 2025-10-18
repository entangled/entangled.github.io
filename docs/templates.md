---
tags:
    - templates
    - mkdocs
    - pandoc
    - new
---

# Project Templates

We provide some project templates to make setting up an Entangled project a bit easier. These templates are powered by [Copier](https://copier.readthedocs.io/en/stable/), for which we provide a front-end with the `entangled new` command. For example:

```bash
entangled new mkdocs ./my-new-project
```

We provide the following templates:

| Handle | URL | Prerequisites | Description |
|---|---|---|---|
| mkdocs | [gh:entangled/template-mkdocs](https://github.com/entangled/template-mkdocs) | python, uv | Fast, simple project documentation for Python |
| pandoc | [gh:entangled/template-pandoc](https://github.com/entangled/template-pandoc) | pandoc | Supports HTML and PDF output, numbered equations and citations. |
| one-pager | [gh:entangled/template-one-pager](https://github.com/entangled/template-one-pager) | none | Everything in a single README that renders well on Github |

The `entangled new` command supports templates from other sources. We welcome contributions for other document generators to add to the list of officially supported templates, but they need to be hosted on the Entangled organisation on Github.

