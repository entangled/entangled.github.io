---
tags:
    - hooks
---

# Hooks

Entangled has a system of *hooks*: these add actions to the tangling process:

- `build` trigger actions in a generated `Makefile`
- `brei` trigger actions (or tasks) using [Brei](https://entangled.github.io/brei), which is automatically installed along with Entangled. This is now prefered over the `build` hook.
- `quarto_attributes` add attributes to the code block in Quatro style with `#|` comments at the top of the code block.
- `repl` (unstable) run REPL sessions using the [`repl-session` command-line tool](https://entangled.github.com/repl-session).
- `shebang` takes the first line if it starts with `#!` and puts it at the top of the file.
- `spdx_license` takes the first line if it contains `SPDX-License-Identifier`
and puts it at the top of the file.


