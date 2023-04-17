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
`sphinx-build` command (which was executed under the hood when we ran `make html`!).

In your virtual environment, install:

```py
pip install sphinx-multiversion
```

and add the extension to `conf.py`:

```py
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.githubpages",
    "sphinx.ext.napoleon",
    "sphinx_multiversion",
]
```

### Configuring the site's sidebar

We want to give our website visitors the option to select which version of the docs they would like
to view. For this reason, we need to configure our sidebar to display them by adding an html
template. Let's create a file `docs/_templates/sidebar/versions.html` that will display a list of
versions (git tags and branches):

{% highlight html %}
{% raw %}
{% if versions %}
<div class="sidebar-tree">
  <p class="caption" role="heading"><span class="caption-text">{{ _('Branches') }}</span></p>
  <ul>
    {%- for item in versions.branches %}
    <li class="toctree-l1"><a class="reference" href="{{ item.url }}">{{ item.name }}</a></li>
    {%- endfor %}
  </ul>
  <p class="caption" role="heading"><span class="caption-text">{{ _('Tags') }}</span></p>
  <ul>
    {%- for item in versions.tags %}
    <li class="toctree-l1"><a class="reference" href="{{ item.url }}">{{ item.name }}</a></li>
    {%- endfor %}
  </ul>
</div>
{% endif %}
{% endraw %}
{% endhighlight %}

Next, we need to point Sphinx to use this file. In `conf.py`, add the following:

```py
templates_path = [
    "_templates",
]
html_sidebars = {
    "**": [
        "sidebar/brand.html",
        "sidebar/search.html",
        "sidebar/scroll-start.html",
        "sidebar/navigation.html",
        "sidebar/versions.html",
        "sidebar/scroll-end.html",
    ],
}
```

We've only added a `versions.html` to `_templates/sidebar/` folder. So where are the other `html`
files coming from? They are the default `sidebar` components that ship with `furo`, namely:

- the website title
- the search bar
- the content tree
- the scrollable section of the sidebar

Now that we are overriding the html_sidebar contents, we need to explicitly tell `furo` we still
want those components in our sidebar in addition to our custom `versions` section.

Finally, we are ready to test this command locally. From the `docs/` dir, run:

```sh
cd docs
make clean
rm -rf _build/html/*  # wipe out the current html folder contents
sphinx-multiversion . ./_build/html
```

You will notice that now, in your `_build/html` folder, there is a subfolder corresponding to each
branch and tag you have in your repo. Since you currently only have the `main` branch, there'll be
one `main` folder in which their corresponding output `html` files live. As you create more
branches and tags, more output `html` folders will be created for them.

Let's commit the changes we have done to support `sphinx-multiversion` to the `main` branch:

```sh
# from the project root
git add docs/conf.py docs/_templates/sidebar/versions.html
git commit -m "Add support for sphinx-multiversion extension"
git push origin main
```

### Generating different versions for your docs

Let's test this out.

Let's tag the current version of our docs with `v0.0.1`. We'll create a tag and push it to our
remote repository:

```sh
git tag -a v0.0.1 -m "first tag"
git push origin v0.0.1
```

Next, make a small change in `index.rst`.

```
Welcome to Sphinxy's documentation!
===================================

Explore Sphinxy's documentation by browsing the different modules.
```

Now, push your changes to `main`:

```sh
git add docs/index.rst
git commit -m "Update docs"
git push origin main
```

Now re-run `sphinx-multiversion`:

```sh
# from the project root
sphinx-multiversion docs ./docs/_build/html
```

You should now see that a new HTML folder was generated: `./docs/_build/html/v0.0.1`. Preview both
`index.html` locally: `./docs/_build/html/main/index.html` and
`./docs/_build/html/v0.0.1/index.html`. As you would expect, the modification will appear on tag
`main`'s `index.html`, but not on `v0.0.1`'s.

You'll also notice the new sidebar section we added titled `Versions`, with links to all available
versions of your documentation.

Finally, let's push the latest contents of the `html/` folder to `gh-pages`:

```sh
cd ./docs/_build/html
git branch # ensure you are on the gh-pages branch
git add .
git commit -m "Add versioning support"
git push -f origin gh-pages
```

Once the `pages-build-deployment` workflow has completed, refresh your GitHub Pages URL. You'll
notice that you'll get a `404 File not found` error. Why is that?

### Choosing a default version

GitHub Pages looks for an `index.html` file at the root level of the `gh-pages` branch. Since we
have been using `sphinx-multiversion` we're storing `index.html` files under their respective git
branch or tag folders now and GitHub could not find a page to load.

Modify your GitHub Pages URL to append a branch or a tag name name:

`https://<username>.github.io/sphinxy/main`

You'll be redirected to that version's index page and everything looks as expected.

Of course, this is not convenient. We want to automatically redirect the user to the latest
version; in our case the version on the `main` branch. In order to achieve that, we need to add an
new file `index.html` at the root of the `gh-pages` branch that has this redirection logic (see the
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
    <link rel="canonical" href="https://<username>.github.io/sphinxy/main/index.html" />
  </head>
</html>
```

Let's commit and push it to the `gh-pages` branch:

```sh
# from the docs/_build/html dir
git add index.html
git commit -m "Add index.html to redirect to main branch"
git push origin gh-pages
```

You should now be automatically redirected to browse the documentation of your `main` branch on
`https://<username>.github.io/sphinxy`!

### Selecting Branches/Tags of Interest

You may not want to include all branches or tags on your documentation website. To configure this
behaviour, add the following to `conf.py`:

```py
# -- Sphinx Multiversion --------------------------------------------------
# https://holzhaus.github.io/sphinx-multiversion/master/configuration.html#
smv_tag_whitelist = r'^v\d+\.\d+\.\d+$'
smv_branch_whitelist = r'^.*$'
smv_remote_whitelist = r'^.*$'
```

Now `sphinx-multiversion` will only generate documentation corresponding to branches and tags
matching your regex expressions above.

{: .tip }
More nuanced settings can be found on
[`sphinx-multiversion` docs](https://holzhaus.github.io/sphinx-multiversion/master/configuration.html#).

{: .hint }
🙌 You have now reached the
[`09-versioning`](https://github.com/aelsayed95/sphinxy/tree/09-versioning) part of the tutorial. If
not, check-out that branch and
[`9-gh-pages`](https://github.com/aelsayed95/sphinxy/tree/9-gh-pages) branch for `gh-pages` and
continue from there.

<br />
[Previous: Automating documentation updates](./8-automating-documentation-updates.md){: .btn .float-left}
[Next: Automating versioning](./10-automating-versioning.md){: .btn .btn-purple .float-right}
<br />
