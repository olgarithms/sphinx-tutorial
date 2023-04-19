---
layout: default
title: 6. Selecting a theme
nav_order: 6
---

# Selecting a theme

---

What if you wanted a different appearance for your website? Sphinx uses the `alabaster` theme by
default, but it also ships with a number of built-in themes you can choose. You can also install
and use a theme from PyPI. We'll cover how to do both in this section.

### Using a built-in theme

First, browse through Sphinx's
[built-in themes](https://www.sphinx-doc.org/en/master/usage/theming.html#builtin-themes) and
select your favourite. We picked the theme `classic`. In `conf.py`, navigate to the section
`Options for HTML output` and edit the `html_theme` variable:

```py
# -- Options for HTML output -------------------------------------------------
# https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output

html_theme = 'classic'
```

Now from the `docs` directory re-build your HTML files:

```sh
# from the docs dir
make html
```

And view `index.html` in your browser, you should be able to see that the theme has changed.

### Using a PyPI Theme

In addition to Sphinx's built-in themes, there are many themes available on PyPI. Our favourite is
[`furo`](https://github.com/pradyunsg/furo). Let's install it:

```sh
pip install furo
```

Now modify `conf.py` again to change the theme:

```py
# -- Options for HTML output -------------------------------------------------
# https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output

html_theme = 'furo'
```

And re-build your and preview your docs:

```sh
make html
```

commit and push your changes:

```sh
# from the project root
git add docs/conf.py
git commit -m "Use furo theme"
git push origin main
```

## Adding a favicon

To finish styling our website, let's see how we can add a favicon.

Sphinx has integrated support for this feature. Find an image you would like to use and move it to
the `_static` directory under `docs`. You could also use the one provided in `images/favicon.ico`.

Then in your `conf.py`, add

```py
html_favicon = "_static/favicon.ico"
```

and run again

```sh
make html
```

The favicon should now appear on your browser tab!

Commit and push your changes:

```sh
# from the project root
git add docs/conf.py docs/_static/favicon.ico
git commit -m "Add favicon"
git push origin main
```

{: .hint }
🙌 You have now reached the
[`06-select-theme`](https://github.com/aelsayed95/sphinxy/tree/06-select-theme)
part of the tutorial. If not, check out that branch and continue from there.

<br />
[Previous: Generating HTML docs from docstrings](./5-generating-docs.md){: .btn .float-left}
[Next: Hosting on GitHub Pages](./7-hosting-on-github-pages.md){: .btn .btn-purple .float-right}
<br />
