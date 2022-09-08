---
layout: default
title: 6. Selecting a theme
nav_order: 6
---

# Selecting a theme

---

What if you wanted a different appearance for your website? Sphinx ships with a number of built-in
themes. You can also install and use a theme from `PyPI`. We'll cover how to do both in this
section.

### Using a built-in theme
First, browse through `Sphinx`'s
[built-in themes](https://www.sphinx-doc.org/en/master/usage/theming.html#builtin-themes) and
select your favorite. We picked the theme `classic`. In `conf.py`, navigate to the section
`Options for HTML output` and edit the `html_theme` variable:

```py
# -- Options for HTML output -------------------------------------------------
# https://www.sphinx-doc.org/en/master/usage/configuration.html#options-for-html-output

html_theme = 'classic'
```

Now from the `docs`directory re-build your HTML files:

```sh
make html
```

And view `index.html` in your browser, you should be able to see that the theme has changed.

### Using a PyPI Theme
In addition to Sphinx's built-in themes, there are many themes available on `PyPI`. Our favorite is
Read The Docs. Let's install [`furo`](https://github.com/pradyunsg/furo):

```sh
python3.10 -m pip install furo
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
```
git add docs/conf.py
git commit -m "Use furo theme!"
git push origin main
```

{: .hint }
🙌 You have now reached the
[`6-selecting-a-theme`](https://github.com/aelsayed95/the-office/tree/6-selecting-a-theme)
part of the tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: Generating HTML docs from docstrings](./generating-docs.md){: .btn .float-left}
[Next: Hosting on GitHub Pages](./hosting-on-github-pages.md){: .btn .btn-purple .float-right}
<br />
