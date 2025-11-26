---
tags:
    - markup
    - markdown
    - typst
    - latex
    - configuration
    - settings
---

# Document Markup Languages

The default setting is to use attributes enclosed in curly braces. This offers a relatively noise-free syntax for denoting code blocks, their labels and file targets. However, not all document generators understand this syntax. 


## Vanilla (Github flavoured) Markdown and Typst

The following setting is compatible with Github's rendering engine, and hence every document generator in existence, including Typst. We use three backtics followed by a language identifier. All other tags are passed using Quarto attributes.
Quarto attributes are prefixed with the configured comment characters, optionally followed by space (some languages require this) and then a pipe symbol `|`. These lines are parsed as YAML.

For example:

~~~markdown
```python
#| id: print-hello
print("Hello, World!")
```

Which needs to be guarded,

```python
#| file: hello.py
if __name__ == "__main__":
    <<print-hello>>
```
~~~

This needs the following `entangled.toml`:

```toml
version="2.0"
hooks=["quarto_attributes"]

[markers]
open="^(?P<indent>\\s*)```(?P<properties>.*)$"
close="^(?P<indent>\\s*)```\\s*$"
```

Or, use the `basic` style setting to the same effect:

```toml
version="2.4"
style="basic"
```

## Not yet implemented: Restructured Text

Restructured text is quite different from Markdown. The following needs an additional hook to parse the attribute section:

~~~rst
.. code-block:: python
    :name: print-hello

    print("Hello, World!")

Which needs to be guarded,

.. code-block:: python
    :name: file:hello.py

    if __name__ == "__main__":
        <<print-hello>>

We need some text here to close the code block.
~~~

And in `entangled.toml`:

```toml
version="2.0"
hooks=["rst_attributes"]   # not yet implemented

[markers]
open="^.. code-block::\\s*(?P<properties>.*)$"
close="^\\S"
```

File an issue and/or PR on Github if you need this.

## Untested: LaTeX

LaTeX can work if we enable Quatro attributes:

~~~latex
\begin{lstlistings}[language=python]
#| id: print-hello
print("Hello, World!")
\end{lstlistings}
~~~

With `entangled.toml`

```toml
version="2.0"
watch_list=["tex/*.tex"]
hooks=["quatro_attributes"]

[markers]
open="^\\\\begin{lstlistings}[language=(?P<properties>[^\\[\\]]*]$"
close="^\\\\end{lstlistings}$"
```

This feels a bit hacky. I think it is better to write your documents in Markdown and use Pandoc to convert to LaTeX.

