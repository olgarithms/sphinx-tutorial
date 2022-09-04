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

Next, we need to point `Sphinx` to this file. In `conf.py`, add the following:

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

Finally, we are ready to test this command locally. From the `docs/` dir, run:

```sh
cd docs
make clean
rm -rf _build/html/*    # empty the gh-pages worktree
sphinx-multiversion . ./_build/html
```

You will notice that now, in your `_build/html` folder, there is a subfolder corresponding to each
branch and tag you have in your repo. For example, there's a `main` folder and an `update_docs`
folder in which their corresponding output `HTML` files live. As you create more branches and tags,
more output `HTML` folders will be created for them.

Let's test this out. Make a small change in `index.rst`. Commit it to `main` and tag it:

```markdown
# Welcome to The Office's documentation!

Explore The Office documentation _by browsing the API documentation_.
```

```sh
git checkout main
git add .
git commit -m "update docs"
git push origin main
git tag -a v0.0.1 -m "new docs version"
git push origin v0.0.1
```

Now re-run `sphinx-multiversion`:

```sh
cd ./docs
sphinx-multiversion . ./_build/html
```

You should now see that a new HTML folder was generated: `./docs_build/html/v0.0.1`. Preview all
three `index.html` locally: `./docs/_build/html/main/index.html`,
`./docs/_build/html/v0.0.1/index.html`, and `./docs/_build/html/update_docs/index.html`. As you'd
expect, the modification will appear in the `main` and the tag `v0.0.1`'s `index.html`, but not in
`update_docs`.

You'll also notice the new sidebar section we added titled `Versions`, with links to all available
versions of your documentation.

Finally, let's push the latest contents of the `html/` folder to `gh-pages`.

```sh
cd ./docs/_build/html/
git add .
git commit -m "versioning support"
git push origin gh-pages
```

Refresh your Github Pages URL. You'll notice that you'll get a `404 File not found` error. Why is
that? gh-pages looks for an `index.html` file at the root level of the `gh-pages` branch. Since
we'd moved we're storing `index.html` files under their respective branch's folders now, Github
could not find a page to load.

Modify your Github Pages URL to append a branch or a tag name name:

`https://<username>.github.io/the-office/main`

You'll be redirected to that version's index page. Of course, this is not convenient. We want to
automatically redirect the user to the latest version. Say, the version on the `main` branch. To do
that, we need to add an new file `index.html` at the root of the `gh-pages` branch that has this
redirection logic. First, we create a new file:

```sh
cd ./docs/_build/html
touch index.html
```

Now, open up this file, and add the following logic:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Redirecting to main branch</title>
    <meta charset="utf-8" />
    <meta http-equiv="refresh" content="0; url=./main/index.html" />
    <link rel="canonical" href="https://<username>.github.io/the-office/main/index.html" />
  </head>
</html>
```

Let's commit and push it to the `gh-pages` branch:

```sh
cd ./docs/_build/html
git add index.html
git commit -m "redirect to main branch"
git push origin gh-pages
```
