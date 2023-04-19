---
layout: default
title: 8. Automating updates
nav_order: 8
---

# Automating documentation updates

---

So far, we've been manually building our documentation files locally, then pushing the output
`html` to GitHub Pages to host it. In this section, we will learn how to automate this process
using GitHub Actions. GitHub Actions allow GitHub users to automatically execute workflows. We can
use this to automate building and deploying new versions of our documentation.

## Creating a simple GitHub Action

First, let's build a simple GitHub Actions workflow to understand how it works. Let's create a
workflow that prints "Hello, World!" when a commit is pushed to `main`:

From your repo's root, create a new file `.github/workflows/hello.yaml` with the following
contents:

```yaml
name: Hello World

on:
  push:
    branches:
      - main

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: say hello
        run: echo "Hello, World!"
```

This file defines a workflow named `Hello World`, which, when triggered, runs a single job `hello`
The job has a single step named `say hello` that prints `Hello, World!` to standard output. To test
it out, commit and push this file to `main`.

```sh
git add .github/workflows/hello.yaml
git commit -m "hello world workflow"
git push origin main
```

Now, navigate to the `Actions` tab on your repo's GitHub URL. You should see that a `workflow` with
the name `Hello World` has run. Click on the workflow, and click on the latest commit. Click on the
job `hello`. Now expand the step `say hello` to see its details. You should see the message
`Hello, World!` printed to stdout.

{: .tip }
You can disable an action by following
[these instructions](https://docs.github.com/en/actions/managing-workflow-runs/disabling-and-enabling-a-workflow).

## Deploying your docs automatically

You now know how to build a simple workflow. Let's now write a workflow that would build and
publish our documentation. Create a new file `.github/workflows/docs.yaml` (find the completed file
[here](#the-complete-deploy-documentation-github-action)):

```yaml
name: Publish Sphinx Documentation

on:
  push:
    branches:
      - main
```

As before, we'll trigger this workflow when a commit is pushed to main. See
[this reference](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
for a full list of events that trigger GitHub workflows. Next, let's define a job
`publish_sphinx_docs` and give `write` permissions to the
[contents scope](https://docs.github.com/en/rest/overview/permissions-required-for-github-apps?apiVersion=2022-11-28#contents):

```yaml
jobs:
  publish_sphinx_docs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
```

Now we can start defining the `steps` that make up our job. Firstly, we need to set up our
environment and we are going to use two actions for this:

- one to check-out our repository, so the workflow can access it
- one to install and set up python with our preferred version

```yaml
steps:
  - uses: actions/checkout@v3
  - uses: actions/setup-python@v3
    with:
      python-version: "3.11"
```

Now we can install our package and the sphinx-related dependencies:

```yaml
- name: Install dependencies
  run: |
    pip install -e .
    pip install sphinx furo
```

Then, we build our Sphinx documentation using `sphinx-apidoc` and `sphinx-build`:

```yaml
- name: Sphinx build
  run: |
    sphinx-apidoc -o docs src/sphinxy/ --separate
    sphinx-build docs docs/_build/html
```

Finally, we deploy our Sphinx documentation to `gh-pages` by using the
[actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) action:

```yaml
- name: Deploy
  uses: peaceiris/actions-gh-pages@v3
  with:
    publish_branch: gh-pages
    github_token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
    publish_dir: docs/_build/html
    force_orphan: true
```

### The complete "Publish Sphinx Documentation" GitHub Action

Putting it all together, your `docs.yaml` should look like this:

```yaml
name: Publish Sphinx Documentation

on:
  push:
    branches:
      - main

jobs:
  publish_sphinx_docs:
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-python@v3
        with:
          python-version: "3.11"
      - name: Install dependencies
        run: |
          pip install -e .
          pip install sphinx furo
      - name: Sphinx build
        run: |
          sphinx-apidoc -o docs src/sphinxy/ --separate
          sphinx-build docs docs/_build/html
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          publish_branch: gh-pages
          github_token: {% raw %}${{ secrets.GITHUB_TOKEN }}{% endraw %}
          publish_dir: docs/_build/html
          force_orphan: true
```

Commit and push your file to `main`:

```sh
# from the project root
git add .github/workflows/docs.yaml
git commit -m "Add documentation build action"
git push origin main
```

From your repo's Github page, go to the `Actions` tab, you should see that the workflow
`Publish Sphinx Documentation` is running. Click on it to observe the details of each step. Once
all steps have completed, you'll see that a new workflow `pages-build-deployment` has kicked off.
This workflow is triggered by GitHub when changes are pushed to `gh-pages`.

When `pages-build-deployment` is completed, refresh your GitHub Pages URL. You should see the new
change on your site.

With this new workflow in place, from now on every commit to `main` is going to automatically
trigger a new deployment of your documentation!

{: .hint }
🙌 You have now reached the
[`08-automate-updates`](https://github.com/aelsayed95/sphinxy/tree/08-automate-updates) part of
the tutorial. If not, check-out that branch and
[`gh-pages-8`](https://github.com/aelsayed95/sphinxy/tree/gh-pages-8) branch for `gh-pages` and
continue from there.

<br />
[Previous: Hosting on GitHub Pages](./7-hosting-on-github-pages.md){: .btn .float-left}
[Next: Versioning](./9-versioning.md){: .btn .btn-purple .float-right}
<br />
