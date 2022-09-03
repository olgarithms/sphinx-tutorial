---
layout: default
title: Generating docs
nav_order: 6
---

# Generating HTML docs from docstrings

---

To build this documentation, we first need to add the `autodoc` extension in our Sphinx setup. Open
`conf.py`, and modify the extensions list:

```py
# Add any Sphinx extension module names here, as strings. They can be
# extensions coming with Sphinx (named 'sphinx.ext.*') or your custom
# ones.
extensions = [
    'sphinx.ext.autodoc'
]
```

Now, we need to run the API doc command `sphinx-apidoc` to build `rst` files out of our
`docstrings`. We need to specify the output path and module path, so from the `docs` directory,
run:

```sh
sphinx-apidoc --output-dir . ..
```

You can see that Sphinx created an `rst` file per module, as well as a `modules.rst` which acts as
a module directory. To link these to our documentation, we'll need to edit `index.rst` to add a
reference to `modules`:

```rst
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules
```

We also need to point Sphinx to the source python module's path. In `conf.py`, edit the
`Path Setup section` and add the parent dir to the path:

```py
import os
import sys
sys.path.insert(0, os.path.abspath('..'))
```

Now, we're ready to build our docs again:

```sh
make html
```

Preview `index.html` on your browser. You should be able to see links to each of the modules.
Clicking on a module should render its API documentation.

Finally, commit and push your documentation src files to your remote repository. To ensure you
don't accidentally commit any generated `html` files, let's first add the `_build` folder to
`~/.gitignore` file to tell `git` to ignore any files under `_build`:

```sh
_build
```

Now from the project root commit and push your documentation src files:

```sh
git add docs .gitignore
git commit -m "Add documentation"
git push origin main
```
