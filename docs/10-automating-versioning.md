---
layout: default
title: 10. Automating versioning
nav_order: 10
---

# Automating versioning

---

We are now ready to integrate `sphinx-multiversion` into our automated deployment workflow. You
probably want to release a new version of your documentation when you've released a new version of
your API.

We'll modify our existing workflow so that it runs on a release, and uses `sphinx-multiversion` to
generate and publish your latest release's documentation (find the completed file
[here](#the-updated-publish-sphinx-documentation-github-action-using-sphinx-multiversion)):.

Open up `.github/workflows/docs.yaml`. First, modify the workflow trigger so that it runs on a new
release:

```yaml
on:
  release:
    types: [published]
```

Since we need to display the content of other git branches and tags, we need to have all history
available. On the step of checking-out our repo, configure the `Checkout V3` action to fetch all
history for all branches and tags:

```yaml
- uses: actions/checkout@v3
  with:
    fetch-depth: 0
```

As expected, you need to install `sphinx_multiversion` in `Install dependencies` step:

```yaml
- name: Install dependencies
  run: |
    pip install -e .
    pip install sphinx furo sphinx_multiversion
```

Next, modify the step `Sphinx build` to use the `sphinx_multiversion` command to build your
documentation:

```yaml
- name: Sphinx build
  run: |
    sphinx-multiversion docs docs/_build/html
```

In the [previous chapter](./9-versioning.md#choosing-a-default-version), we saw how we need to
choose a default version to redirect to the `main` branch and we need to include it in our action.
Copy the logic of `index.html` to a file called `gh-pages-redirect.html` under `docs/_templates/`
and commit it to your `main` branch. This way we ensure we have a template ready to be copied every
time we regenerate our docs with the action.

```sh
# from the project root
cp ./docs/_build/html/index.html docs/_templates/gh-pages-redirect.html
git add docs/_templates/gh-pages-redirect.html
git cm "Add a template for gh-pages-redirect"
git push origin main
```

Then add another step to your action to create an `index.html`:

```yaml
- name: Create index.html
run: |
    cp docs/_templates/gh-pages-redirect.html docs/_build/html/index.html
```

### The updated "Publish Sphinx Documentation" GitHub Action using sphinx-multiversion

Your file `.github/workflows/docs.yaml` should now look like this:

```yaml
name: Publish Sphinx Documentation

on:
  release:
    types: [published]

jobs:
  publish_sphinx_docs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - uses: actions/setup-python@v3
        with:
          python-version: "3.11"
      - name: Install dependencies
        run: |
          pip install -e .
          pip install sphinx furo sphinx_multiversion
      - name: Sphinx build
        run: |
          sphinx-multiversion docs docs/_build/html
      - name: Create index.html
        run: |
          cp docs/_templates/gh-pages-redirect.html docs/_build/html/index.html
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          publish_branch: gh-pages
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: docs/_build/html
          force_orphan: true
```

Now, commit your updated workflow to `main`:

```sh
# from the project root
git add .github/workflows/docs.yaml
git commit -m "Use sphinx-multiversion in docs action"
git push origin main
```

To test this, make a small change to `docs/index.rst`, commit and push it to `main`.

```
Welcome to Sphinxy's documentation!
===================================

Explore Sphinxy's documentation by browsing the different modules.

Choose one from below.
```

```bash
git add docs/index.rst
git commit -m "Update index doc"
git push origin main
```

From your repo's GitHub page, hit the link "Create a new release". From the tags dropdown menu,
write down the next tag version to create `v0.0.2`. Write a brief description and hit
`Publish Release`.

From the `Actions` tab on your repo's GitHub URL, you should see that the
`Publish Sphinx Documentation` workflow has kicked off. Inspect the job details to verify that
`sphinx-multiversion` has run.

Now, refresh your GitHub Pages URL. You should see your update has taken place on the `main`
version as well as `v0.0.2` of your docs.

{: .hint }
🙌 You have now reached the
[`10-automating-versioning`](https://github.com/aelsayed95/sphinxy/tree/10-automating-versioning)
part of the tutorial. If not, check-out that branch and
[`10-gh-pages`](https://github.com/aelsayed95/sphinxy/tree/10-gh-pages) branch for `gh-pages` and
continue from there.

🎉 **Congratulations!** 🎉

We are done! We recommend you take a look at the [Further reading](#further-reading) section if you
want to learn more about documentation generation.

<br />
[Previous: Versioning](./9-versioning.md){: .btn .float-left}
[Next: Glossary](./11-glossary.md){: .btn .btn-purple .float-right}
<br />
