---
layout: default
title: 5. Generating documentation
nav_order: 5
---

# Generating HTML docs from docstrings

---

To build this documentation, we first need to add some extensions in our Sphinx setup.

- [`autodoc`](https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html) pulls
  documentation from docstrings
- [`napoleon`](https://www.sphinx-doc.org/en/master/usage/extensions/napoleon.html) enables Sphinx
  to parse both NumPy and Google style docstrings

Open `conf.py`, and modify the extensions list:

```py
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.napoleon",
]
```

Now, we need to run the command `sphinx-apidoc` to build `rst` files out of our `docstrings`. We
need to specify the output path and module path, so from the root of your repo, run:

```sh
# from the project root
sphinx-apidoc --output-dir docs src/sphinxy --separate
```

You can see that Sphinx created an `rst` file for the package, two `rst` files for each of the
modules `riddle` and `sphinx`, as well as a `modules.rst` file, which acts as a module directory.

{: .tip }
You do not need to re-run `sphinx-apidoc` to generate new `.rst` if you modify their
corresponding `.py` files! You'd only ever need to re-run `sphinx-apidoc` if you add more modules
to your project.

Next, we are going to edit `index.rst` to add a reference to `modules`, so that it appears in the
[table of contents](restructuredtext-basics.md#table-of-contents-tree):

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:

   modules
```

Now, we're ready to build our docs again:

```sh
# from the docs dir
make html
```

Preview `index.html` on your browser. You should be able to see links to each of the modules.
Clicking on a module should render its API documentation.

{: .tip }
Instead of manually running `make html` every time you update your docs, you can use the
extension `sphinx.autobuild`. This will automatically rebuild your documentation on file save and
auto-reload the HTML pages in the browser. Learn more
[here](https://pypi.org/project/sphinx-autobuild/).

Finally, commit and push your documentation src files to your remote repository. To ensure you
don't accidentally commit any generated `html` files, add the `_build/` directory to a `.gitignore`
file at the root of your repository, so that these files are ignored by `git`:

```sh
_build/
```

Now commit and push your documentation src files:

```sh
# from the project root
git add docs .gitignore
git commit -m "Add documentation"
git push origin main
```

{: .hint }
🙌 You have now reached the
[`5-generating-documention`](https://github.com/aelsayed95/sphinxy/tree/5-generating-documention)
part of the tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: Documenting your API](./4-documenting-your-api.md){: .btn .float-left}
[Next: Selecting a theme](./6-selecting-a-theme.md){: .btn .btn-purple .float-right}
<br />
