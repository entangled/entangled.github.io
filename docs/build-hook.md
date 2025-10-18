# `build` hook

!!! warning "Depricated"

    In the near future, the `build` and `brei` hooks will merge so that you can configure the backend of a workflow. The Markdown syntax will follow that of the current `brei` hook.

You can enable this hook in `entangled.toml`:

```toml
version = "2.0"
watch_list = ["docs/**/*.md"]
hooks = ["build"]
```

Then in your Markdown, you may enter code tagged with the `.build` tag.

~~~markdown
``` {.python .build target=docs/fig/plot.svg}
from matplotlib import pyplot as plt
import numpy as np

x = np.linspace(-np.pi, np.pi, 100)
y = np.sin(x)
plt.plot(x, y)
plt.savefig("docs/fig/plot.svg")
```
~~~

This code will be saved into a Python script in the `.entangled/build` directory, or if you specify the `file=` attribute some other location. Second, a `Makefile` is generated in `.entangled/build`, that can be invoked as,

```shell
make -f .entangled/build/Makefile
```

You may configure how code from different languages is evaluated in `entangled.toml`. For example, to add Gnuplot support, and also make Julia code run through `DaemonMode.jl`, you may do the following:

```toml
[hook.build.runners]
Gnuplot = "gnuplot {script}"
Julia = "julia --project=. --startup-file=no -e 'using DaemonMode; runargs()' {script}"
```

Once you have the code in place to generate figures and markdown tables, you can use the syntax at your disposal to include those into your Markdown. In this example that would be

~~~markdown
![My awesome plot](fig/plot.svg)
~~~

In the case of tables or other rich content, Standard Markdown (or CommonMark) has no syntax for including other Markdown files, so you'll have to check with your own document generator how to do that. In MkDocs, you could use `mkdocs-macro-plugin`, Pandoc has `pandoc-include`, etc.

You can also specify intermediate data generation like so:

~~~markdown
``` {.python .build target="data/result.csv"}
import numpy as np
import pandas as pd

result = np.random.normal(0.0, 1.0, (100, 2))
df = pd.DataFrame(result, columns=["x", "y"])
df.to_csv("data/result.csv")
```

``` {.python .build target="fig/plot.svg" deps="data/result.csv"}
import pandas as pd

df = pd.read_csv("data/result.csv")
plot = df.plot()
plot.savefig("fig/plot.svg")
```
~~~

The snippet for generating the data is given as a dependency for that data; to generate the figure, both `result.csv` and the code snippet are dependencies.


