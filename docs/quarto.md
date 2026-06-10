---
title: Quarto
---

[Quarto](https://quarto.org/) is a publishing system for scientific papers and technical reports. It provides code-block evaluation integration for languages that support Jupyter, specifically R, Python and Julia are supported.

The syntax for codeblocks in Quarto is compatible with Entangled. Using Quarto and Entangled together lets you publish reusable software packages integrated with a demonstration of their use in a single notebook. An example is shown at [entangled/quarto-example](https://entangled.github.io/quarto-example).

## Remarks

Quarto will evaluate code block by default, so I had to use the following Markdown header:

```markdown
---
title: "Spruce Budworms"
subtitle: "reproducing some results in May'77"
jupyter: python3
execute:
  eval: false
bibliography: references.bib
default-image-extension: svg
---
```

And the `entangled.toml` as follows:

```toml
version="2.4"
style="default"
hooks=["quarto_attributes"]
watch_list=["*.qmd"]
```

Code blocks that do need to be evaluated by Jupyter are then marked with `eval: true` and often `code-fold: true`.

Quarto accepts a `filename` attribute on codeblocks to set a title. I used this to set the title of eval blocks to `>>>` and Entangled blocks to their id/target. This is not automated and feels a bit odd. In fact, I'd like to give these blocks different colors, so that it is more obvious what code ends up in the package and what is used to run the notebook session.

A few minor changes to Entangled or to Quarto could make them integrate better, reusing more meta-data from one or the other.

To render the file to HTML, I use the following command:

```bash
uv run quarto render may77.qmd --to html --toc --output-dir docs -o index.html
```

This saves the rendered output to `docs/index.html`.
