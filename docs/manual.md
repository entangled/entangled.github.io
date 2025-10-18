# User Manual

## Introduction
Entangled helps you write Literate Programs in Markdown. You put all your code inside Markdown code blocks. Entangled automatically extracts the code and writes it to more traditional source files. You can then edit these generated files, and the changes are being fed back to the Markdown.

``` {.bash .eval}
entangled --version
```

We refer to the Markdown files as 'source' and the traditional source code as 'targets'. Just to keep thigs confusing ;).

## Command-line
The entangled command-line tool has several sub-commands.

``` {.bash .eval}
entangled --help
```

The command you'll be using most often is `watch`. To understand what the watch daemon does, it is instructive to first see what the other commands do.

### Tangle
The `tangle` command extracts code blocks from the markdown and writes them to their respective destination.

``` {.bash .eval}
entangled tangle --help
```

### Stitch
The `stitch` command extracts from the contents of the database any of the input markdown files. If a target file was changed and inserted, the contents will be different from those on the file system. The result is written to standard output.

``` {.bash .eval}
entangled stitch --help
```

### Sync
The Sync command inspects all relevant files and determines if either tangling or stitching needs to happen.

``` {.bash .eval}
entangled sync --help
```

### Watch
This starts a loop waiting for events on your filesystem. If any of the involved files is written to, the daemon will run the equivalent of `entangled sync`.

``` {.bash .eval}
entangled watch --help
```

# Standard syntax
The standard syntax is aimed to work well together with Pandoc. Every code block is delimited with three back ticks. Added to the opening line is a sequence of space separated **code properties**. These properties align with the CSS attributes that would end up in the generated HTML. For those unfamiliar with CSS:

- `#identifier`; a name prefixed with a `#` (sharp), identifies the object, only one of these should be present per item
- `.class`; a name prefixed with `.` (period), assigns the object to a class, a object can belong to any number of classes
- `key=value`; a name suffixed with `=` (equals), optionally followed by a value, adds any meta-data attribute to the object

The complete syntax of a code block then looks like:

~~~markdown
 ``` {[#<reference>|.<language>|<key>=<value>] ...}
 <code> ...
 ```
~~~

The first class in the code properties is always interpreted to give the **programming language** of the code block. In Entangled, any code block is one of the following:
    
- A **referable** block: has exactly one **reference id** (`#<reference>`) and a class giving the language of the code block (`.<language>`). Example:

  ~~~markdown
  ``` {.rust #hello-rust}
  println!("Hello, Rust!");
  ```
  ~~~
 
  In some cases you may want to have additional classes to trigger hooks or a specific filter. Always the first class is interpreted to identify the language.
- A **file** block: has a key-value pair giving the path to the file (`file=<path>`), absolute or relative to the directory in which `entangled` runs. Again, there should be one class giving the language of the block (`.<language>`). Example:

  ~~~markdown
  ``` {.rust file=src/main.rs}
  fn main() {
     <<hello-rust>>
  }
  ```
  ~~~

  The identifier in a file block is optional. If it is left out, the identifier will silently be taken to be the file name.
- An **ignored** block: anything not matching the previous two.

## Tangle rules
Entangled recognizes three parameters for every code block. Since version 1.2 the way these parameters are extracted from an opening line may be [configured using regular expressions](#custom-syntax). Formally, every code block has three properties:

- `Language`
- `Identifier`
- `Optional Filename`

Entangled should tangle your code following these rules:

1. If an **identifier** is repeated the contents of the code blocks is concatenated in the order that they appear in the Markdown. If an identifier appears in multiple files, the order is dependent on the order by which the files appear in the configuration, or if they result from a glob-pattern expansion, alphabetical order.

3. **Noweb references** are **expanded**. A noweb reference in Entangled should occupy a single line of code by itself, and is enclosed with double angle brackets, and maybe indented with white space. Space at the end of the line is ignored.

   ~~~txt
   +--- indentation ---+--- reference  ---+--- possible space ---+
                       <<noweb-reference>>
   ~~~

   The reference is expanded recursively, after which the indentation is prefixed to every line in the expanded reference content.

4. **Annotation**; Expanded and concatenated code blocks are annotated using comment lines. These lines should not be touched when editing the generated files. The default method of annotation follows an opening comment with `~\~ begin <<filename#identifier>>[n]`, and a closing comment with `~\~ end`. For example

   ~~~rust
   // ~\~ begin <<lit/index.md|main>>[1]
   println!("hello");
   // ~\~ end
   ~~~


