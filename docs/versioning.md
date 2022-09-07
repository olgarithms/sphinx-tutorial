---
layout: default
title: 9. Versioning
nav_order: 9
---

# Versioning

---

As your project evolves and new features are added, you may be supporting different versions of
your code. For this reason, it is a good idea to keep an archive of different versions of your
documentation, so your users can refer to it depending on the software version they are using.

Let's see how you can achieve that.

## Using `sphinx-multiversion`

We will be using [`sphinx-multiversion`](https://holzhaus.github.io/sphinx-multiversion/master/), a
popular extension that makes versioning your documentation easy. This will replace the traditional
`sphinx-build` command (which was executed under the hood when we ran `make html`!)

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
versions (git tags and branches) on the website sidebar:

{% highlight html %}
{% raw %}
{% if versions %}
<h3>{{ _('Branches') }}</h3>
<ul>
  {%- for item in versions.branches %}
  <li><a href="{{ item.url }}">{{ item.name }}</a></li>
  {%- endfor %}
</ul>
<h3>{{ _('Tags') }}</h3>
<ul>
  {%- for item in versions.tags %}
  <li><a href="{{ item.url }}">{{ item.name }}</a></li>
  {%- endfor %}
</ul>
{% endif %}
{% endraw %}
{% endhighlight %}

Next, we need to point `Sphinx` to use this file. In `conf.py`, add the following:

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
rm -rf _build/html/*    # wipe out the current html folder contents
sphinx-multiversion . ./_build/html
```

You will notice that now, in your `_build/html` folder, there is a subfolder corresponding to each
branch and tag you have in your repo. For example, there's a `main` folder and an `update_docs`
folder in which their corresponding output `HTML` files live. As you create more branches and tags,
more output `HTML` folders will be created for them.

### Generating different versions for your docs

Let's test this out. Make a small change in `index.rst`.

```
Welcome to The Office's documentation!
======================================

Explore The Office documentation by browsing the different modules.
```

Commit it to `main` and tag it:

```sh
git checkout main
git add docs
git commit -m "update docs"
git push origin main
git tag -a v0.0.1 -m "new docs version"
git push origin v0.0.1
```

Now re-run `sphinx-multiversion`:

```sh
sphinx-multiversion docs ./docs/_build/html
```

You should now see that a new HTML folder was generated: `./docs/_build/html/v0.0.1`. Preview all
three `index.html` locally: `./docs/_build/html/main/index.html`,
`./docs/_build/html/v0.0.1/index.html`, and `./docs/_build/html/update_docs/index.html`. As you'd
expect, the modification will appear in the `main` and the tag `v0.0.1`'s `index.html`, but not in
`update_docs`.

You'll also notice the new sidebar section we added titled `Versions`, with links to all available
versions of your documentation.

Finally, let's push the latest contents of the `html/` folder to `gh-pages`:

```sh
cd ./docs/_build/html
git branch # ensure you are on the gh-pages branch
git add .
git commit -m "versioning support"
git push origin gh-pages
```

Refresh your Github Pages URL. You'll notice that you'll get a `404 File not found` error. Why is
that?

### Choosing a default version

`Github Pages` looks for an `index.html` file at the root level of the `gh-pages` branch. Since we
have been using `sphinx-multiversion` we're storing `index.html` files under their respective git
branch or tag folders now and Github could not find a page to load.

Modify your Github Pages URL to append a branch or a tag name name:

`https://<username>.github.io/the-office/main`

You'll be redirected to that version's index page. Of course, this is not convenient. We want to
automatically redirect the user to the latest version. Say, the version on the `main` branch. To do
that, we need to add an new file `index.html` at the root of the `gh-pages` branch that has this
redirection logic (see the
[docs](https://holzhaus.github.io/sphinx-multiversion/master/github_pages.html?highlight=meta%20http%20equiv%20refresh#redirecting-from-the-document-root)).
First, we create a new file:

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

<!-- prettier-ignore -->
{: .hint }
🙌 You have now reached the
[`9-versioning`](https://github.com/aelsayed95/the-office/tree/9-versioning) part of the tutorial.
If not, check-out that branch and
[`9-gh-pages`](https://github.com/aelsayed95/the-office/tree/9-gh-pages) branch for `gh-pages` and
continue from there.

<br />
[Previous: Automating documentation updates](./automating-documentation-updates.md){: .btn .float-left .mb-lg-4}
[Next: Automating versioning](./automating-versioning.md){: .btn .btn-purple .float-right .mb-lg-4}
<br />
