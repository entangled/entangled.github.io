# Welcome to Entangled
![PyPI - Version](https://img.shields.io/pypi/v/entangled-cli)

**Entangled** lets you write code and proze in a single document. It extracts working source code from your documentation, and keeps the documentation and source code mutualy synchronized. Entangled works with any programming language and supports a wide variety of markup languages, principally most dialects of Markdown.

<section markdown="1" style="display: flex">
<div markdown="1" style="flex-basis:50%; padding: 20pt">

!!! note "Why Entangled"
   
    You keep on using:

    - your favourite **editor**: Entangled runs as a daemon in the background, keeping your text files synchronised.
    - your favourite **programming language**: Entangled is agnostic to programming languages.
    - your favourite **document generator**: Entangled is configurable to any dialect of Markdown.

</div>
<div markdown="1" style="flex-basis:50%; padding: 20pt">

!!! note "Quotes"

    > "A critical aspect of a programming language is the means it provides
    for using names to refer to computational objects." [Abelson, Sussman & Sussman - SICP](https://mitpress.mit.edu/sites/default/files/sicp/index.html)

    ---

    > "Let us change our traditional attitude to the construction of programs:
    > Instead of imagining that our main task is to instruct a computer what to do,
    > let us concentrate rather on explaining to human beings what we want a
    > computer to do." [Knuth - Literate Programming](http://www.literateprogramming.com/knuthweb.pdf)

    ---

    > “Talk is cheap. Show me the code.” [Linus Torvalds]()

</div>
</section>

## Get Started
With the 2.0 release, Entangled is now available as a Python package. To install Entangled, all you need is a Python (version &ge;3.12) installation. 

```bash
pip install entangled-cli
```

<script async id="asciicast-591604" src="https://asciinema.org/a/591604.js"
        data-autoplay="true" data-speed="2"></script>

## About
Entangled helps you write Literate Programs in Markdown. You put all your code inside Markdown code blocks. Entangled automatically extracts the code and writes it to more traditional source files. You can then edit these generated files, and the changes are being fed back to the Markdown. In this way Entangled offers a two-way synchronisation mechanism and ensures that your Markdown files stay up-to-date with your code and vice-versa.

### License
Entangled is the brain child of Johan Hidding, developed with some support from the Netherlands eScience Center, licensed under [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0).

### Citation
If you use Entangled in your research, please cite the following items:

- [Entangled, a Bidirectional System for Sustainable Literate Programming (doi:10.1109/e-Science58273.2023.10254816)](https://doi.org/10.1109/e-Science58273.2023.10254816)
- [Entangled on Zenodo (doi:10.5281/zenodo.13305230)](https://doi.org/10.5281/zenodo.13305230)

Sadly the first reference is behind a pay-wall, but you can read the [full text here](ieee-escience-paper.md).

We are trying to increase the visibility of Entangled. If you like Entangled, please consider adding this badge [![Entangled](https://img.shields.io/badge/entangled-Use%20the%20source!-%2300aeff)](https://entangled.github.io/) to the appropriate location in your project:

```markdown
[![Entangled badge](https://img.shields.io/badge/entangled-Use%20the%20source!-%2300aeff)](https://entangled.github.io/)
```

## Basic Use

Run the `entangled watch` daemon in the root of your project folder. By default all Markdown files are monitored for fenced code blocks like so:

~~~markdown
``` {.rust #hello file="src/world.rs"}
...
```
~~~

The syntax of code block properties is the same as CSS properties: `#hello` gives the block the `hello` identifier, `.rust` adds the `rust` class and the `file` attribute is set to `src/world.rs` (quotes are optional). For Entangled to know how to tangle this block, you need to specify a language and a target file. However, now comes the cool stuff. We can split our code in meaningful components by cross-refrences.

### Hello World

The combined code-blocks in this example compose a compilable source code for "Hello World". For didactic reasons we don't always give the listing of an entire source file in one go. In stead, we use a system of references known as *noweb* (after Ramsey 1994).

Inside source fragments you may encounter a line with `<<...>>` marks like,

~~~markdown
``` {.cpp file=hello_world.cc}
#include <cstdlib>
#include <iostream>

<<example-main-function>>
```
~~~

which is then elsewhere specified. Order doesn't matter,

~~~markdown
``` {.cpp #hello-world}
std::cout << "Hello, World!" << std::endl;
```
~~~

So we can reference the `<<hello-world>>` code block later on.

~~~markdown
``` {.cpp #example-main-function}
int main(int argc, char **argv)
{
    <<hello-world>>
}
```
~~~

A definition can be appended with more code as follows (in this case, order does matter!):

~~~markdown
``` {.cpp #hello-world}
return EXIT_SUCCESS;
```
~~~

These blocks of code can be *tangled* into source files.

### Configuring

Entangled is configured by putting a `entangled.toml` in the root of your project.

```toml
# required: the minimum version of Entangled
version = "2.0"            

# default watch_list is ["**/*.md"]
watch_list = ["docs/**/*.md"]
```

You may add languages as follows:

```toml
[[languages]]
name = "Java"
identifiers = ["java"]
comment = { open = "//" }

# Some languages have comments that are not terminated by
# newlines, like XML or CSS.
[[languages]]
name = "XML"
identifiers = ["xml", "html", "svg"]
comment = { open = "<!--", close = "-->" }
```

The `identifiers` are the tags that you may use in your code block header to identify the language. Using the above config, you should be able to write:

~~~markdown
``` {.html file=index.html}
<!DOCTYPE html>
<html lang="en">
    <<header>>
    <<body>>
</html>
```

And so on...
~~~


