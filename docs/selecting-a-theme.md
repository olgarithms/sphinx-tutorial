---
layout: default
title: Selecting a theme
nav_order: 7
---

# Selecting a theme

---

What if you wanted a different appearance for your website? Sphinx ships with a number of built-in themes. You can also install and use a theme from `PyPI`. We'll cover how to do both in this section.

First, browse through `sphinx`'s [built-in themes](https://www.sphinx-doc.org/en/master/usage/theming.html#builtin-themes) and select your favorite. We picked the theme `classic`. In `conf.py`, navigate to the section `Options for HTML output` and edit the `html_theme` variable:

```py
# -- Options for HTML output -------------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
#
html_theme = 'classic'
```

Now re-build your html:

```sh
make html
```

And view `index.html` in your browser, you should be able to see that the theme has changed.

In addition to Sphinx's built-in themes, there are many themes available on `PyPI`. Our favorite is
Read The Docs. Let's install [`furo`](https://github.com/pradyunsg/furo):

```sh
python3.10 -m pip install furo
```

Now modify `conf.py` again to change the theme:

```py
# -- Options for HTML output -------------------------------------------------

# The theme to use for HTML and HTML Help pages.  See the documentation for
# a list of builtin themes.
#
html_theme = 'furo'
```

And re-build your docs:

```sh
make html
```

---
