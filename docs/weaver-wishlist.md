---
title: Wishlist
---

Entangled should work with most Markdown based documentation systems, although it is not always easy to make the output look seamless. Ideally, we should have a support page here for many different engines, containing instructions on how to get the most out of them.

- [mdBook](https://rust-lang.github.io/mdBook/)
- [Astro Starlight](https://starlight.astro.build/)

## Language specific engines

### Julia

<div class="grid" markdown>
- [Documenter.jl](https://documenter.juliadocs.org/stable/) Works well with the `basic` attribute style. If you want to use `default` attribute style, you need to add some tweaks in `docs/make.jl`. I end up transpiling the Markdown to a separate folder and then running Documenter on that. See [CarboKitten.jl](https://mindthegap-erc.github.io/CarboKitten.jl/) for an example. There should be a more elegant way of integrating Entangled into Documenter, but I never had the time to figure this out.

!!! note "CarboKitten.jl"
    CarboKitten is a toolkit for modelling carbonate platforms stratigraphy.
    [![](https://mindthegap-erc.github.io/CarboKitten.jl/dev/fig/glamour_view.png)](https://mindthegap-erc.github.io/CarboKitten.jl/dev/)
</div>

### Python

- [Sphinx](https://www.sphinx-doc.org/en/master/) was the defacto standard documentation tool for Python for a long time and still much in use. It comes with a markup format known as reStructuredText, which is a bit more strict than Markdown, but also quite terrible to look at. Sphinx does have plugins that let you write documentation in Markdown though, and this way it can be made to work together well with Entangled.

### Racket

- [Scribble](https://docs.racket-lang.org/scribble/) has support for literate programming build-in, but I think this only applies to Racket native code. Scribble has `@verbatim|{ ... }|` tags that Entangled could be made to trigger on. You could then use Racket to code the document presentation while providing code in an entirely different language, similar to how you can use Typst.

### Rust

- [rustdoc](https://doc.rust-lang.org/rustdoc/what-is-rustdoc.html)
