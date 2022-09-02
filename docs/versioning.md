---
layout: default
title: Versioning
nav_order: 10
---

# Versioning

---

As your project evolves and new features are added, you may be supporting different versions of
your code. In those cases it is a good idea to keep an archive of different versions of your
documentation as well, so your users can refer to it depending on the software version they are
using.

Let's see how you can achieve that.

We will be using
[`sphinx-multiversion`](https://holzhaus.github.io/sphinx-multiversion/master/quickstart.html), a
popular extension that makes versioning your documentation easy.

In your virtual environment, install:

```py
python3.10 -m pip install sphinx-multiversion
```

and add the extension to `conf.py`:

```py
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.githubpages",
    "sphinx_multiversion"
]
```

Next, to render a list of links to past versions on your Documentation website, you'll need to add
an HTML template. Let's create a file `docs/_templates/versioning.html` that will display a list of
github tags and versions on the website side bar:

```html
{% if versions %}
<h3>{{ _('Versions') }}</h3>
<ul>
  {%- for item in versions %}
  <li><a href="{{ item.url }}">{{ item.name }}</a></li>
  {%- endfor %}
</ul>
{% endif %}
```

Next, we need to point `sphinx` to this file. In `conf.py`, add the following:

```py
templates_path = [
    "_templates",
]
html_sidebars = {
    '**': [
        'versioning.html',
    ],
}
```

Finally, we are ready to test this command locally. From the project root directory run:

```sh
sphinx-multiversion docs _build
```

You will notice that that in your `_build` folder now, there is a `main` folder under which the
output `HTML` lives. As you create tags or branches in your repo, `sphinx-multiversion` will create
folders with output HTML files corresponding to the source documentation files in their respective
branches.

Let's test this out. Make a small change in `index.rst`. Commit it to `main` and tag it:

```sh
git checkout main
git add `docs/index.rst`
git commit -m "update docs"
git push origin main
git tag -a v0.0.1 -m "new docs version"
git push origin v0.0.1
```

Now re-run `sphinx-multiversion`:

```sh
sphinx-multiversion docs _build
```

You should now see two folders under `_build/`: `main` and `v0.0.1`.

---
