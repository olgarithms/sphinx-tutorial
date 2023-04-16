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
generate and publish your latest release's documentation.

Open up `.github/workflows/docs.yaml`. First, modify the workflow trigger so that it runs on a new
release:

```yaml
on:
  release:
    types: [published]
```

Next, modify the step `Install dependencies` to install `sphinx_multiversion`:

```yaml
- name: Install dependencies
  run: |
    pip install sphinx furo sphinx_multiversion
```

Next, modify the step `Sphinx build` to use the `sphinx_multiversion` command to build your
documentation:

```yaml
- name: Sphinx build
  run: |
    git worktree add docs/_build/html gh-pages
    cd docs
    sphinx-multiversion . ./_build/html
```

Your file `.github/workflows/docs.yaml` should now look like this:

```yaml
name: Deploy Documentation

on:
  release:
    types: [published]

jobs:
  deploy_docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-python@v3
        with:
          python-version: "3.11"
      - name: Install dependencies
        run: |
          pip install sphinx furo sphinx_multiversion
      - uses: actions/checkout@v3
        with:
          fetch-depth: 0
      - name: Git config
        run: |
          git config user.email "your email"
          git config user.name "your name"
      - name: Sphinx build
        run: |
          git worktree add docs/_build/html gh-pages
          cd docs
          sphinx-multiversion . ./_build/html
      - name: Deploy docs
        run: |
          cd docs/_build/html
          git add --all
          git commit -m "deploy documentation updates"
          git push origin gh-pages
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

Choose one from below:
```

```bash
git add docs/index.rst
git commit -m "Update index doc"
git push origin main
```

From your repo's GitHub page, hit the link "Create a new release". From the tags dropdown menu,
write down the next tag version to create `v0.0.2`. Write a brief description and hit
`Publish Release`.

From the `Actions` tab on your repo's GitHub URL, you should see that the `Deploy Documentation`
workflow has kicked off. Inspect the job details to verify that `sphinx-multiversion` has run.

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
