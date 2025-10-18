---
tags:
    - attributes
---

# `quarto_attributes` hook

Sometimes using the `build` hook (or the `brei` hook, see below), leads to long header lines. It is then better to specify attributes in a header section of your code. The Quarto project came up with a syntax, having the header be indicated by a comment with a vertical bar, e.g. `#|` or `//|` etc. The `quarto_attributes` hook reads those attributes and adds them to the properties of the code block.

Example with the `brei` hook:

~~~markdown
``` {.python .task}
#| description: Draw a triangle
#| creates: docs/fig/triangle.svg
#| collect: figures
from matplotlib import pyplot as plt
plt.plot([[-1, -0.5], [1, -0.5], [0, 1], [-1, -0.5]])
plt.savefig("docs/fig/triangle.svg")
```

![](fig/triangle.svg)
~~~

Using these attributes it is possible to write in Entangled using completely standard Markdown syntax. 

The `id` attribute is reserved for the code's identifier (normally indicated with `#`) and the `classes` attribute can be used to indicate a list of classes in addition to the language class already given.


