---
layout: default
title: 8. Automating updates
nav_order: 8
---

# Automating documentation updates

---

So far, we'd been manually building our documentation files locally, then pushing the output `html`
to `Github Pages` to host it. In this section, we will learn how to automate this process using
`Github Actions`. `Github Actions` allow `Github` users to automatically execute workflows. We can
use this to automate building and deploying new versions of our documentation.

## Creating a simple Github Action

First, let's build a simple `Github Actions` workflow to understand how it works. Let's create a
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
The job has a single step named `say hello` that prints `Hello, Wolrd!` to stdout. To test it out,
commit and push this file to `main`.

```sh
git add ./.github/workflows/hello.yaml
git commit -m "hello world workflow"
git push origin main
```

Now, navigate to the `Actions` tab on your repo's Github URL. You should see that a `workflow` with
the name `Hello World` has run. Click on the workflow, and click on the latest commit. Click on the
job `hello`. Now expand the step `say hello` to see its details. You should see the message
`Hello, World!` printed to stdout.

## Deploying your docs automatically

You now know how to build a simple workflow. Let's now write a workflow that would build and deploy
our documentation when a PR is merged. Create a new file `.github/workflows/docs.yaml` (find the
full file [here](automating-documentation-updates?plain=1#L129)):

```yaml
name: Deploy Documentation

on:
  pull_request:
    types:
      - closed
```

We'll trigger this workflow when a `pull_request` "closed" event occurs - meaning when a PR is
closed. See
[this reference](https://docs.github.com/en/actions/using-workflows/events-that-trigger-workflows)
for a full list of events that trigger Github workflows. Next, let's define a job, we'll call it
`deploy docs`. We only want to trigger our workflow if a PR is merged, so we add the following
condition:

```yaml
jobs:
  deploy_docs:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
```

Next, we need to define the `steps` that make up our job. First we setup Python, choose the
version, and install required dependencies:

```yaml
steps:
  - uses: actions/setup-python@v3
    with:
      python-version: "3.10"
  - name: Install dependencies
    run: |
      pip install sphinx furo
```

Next, we checkout the current repo, and configure Github to allow us to push to the `gh-pages`
branch:

```yaml
- uses: actions/checkout@v3
  with:
    fetch-depth: 0
- name: Git config
  run: |
    git config user.email "your email"
    git config user.name "your name"
```

Finally, we build our Sphinx documentation using `sphinx-build` (run by `make html`) and push the
output `_build/html` folder to `gh-pages`:

```yaml
- name: Sphinx build
  run: |
    git worktree add docs/_build/html gh-pages
    cd docs
    make html
- name: Deploy docs
  run: |
    cd docs/_build/html
    git add --all
    git commit -m "deploy documentation updates"
    git push origin gh-pages
```

Putting it all together, your `docs.yaml` should look like this:

```yaml
name: Deploy Documentation

on:
  pull_request:
    types:
      - closed

jobs:
  deploy_docs:
    if: github.event.pull_request.merged == true
    runs-on: ubuntu-latest
    steps:
      - uses: actions/setup-python@v3
        with:
          python-version: "3.10"
      - name: Install dependencies
        run: |
          pip install sphinx furo
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
          make html
      - name: Deploy docs
        run: |
          cd docs/_build/html
          git add --all
          git commit -m "deploy documentation updates"
          git push origin gh-pages
```

Commit and push your file to `main`. To test out our new command, make a quick change to
`docs/index.rst`:

```
Welcome to The Office's documentation!
======================================

Explore The Office documentation.
```

Commit your change to a new branch `update_docs`:

```sh
git checkout -b update_docs
git add docs/index.rst
git commit -m "update docs"
git push origin update_docs
```

Now, create a PR to merge `update_docs` into `main`. As this is a forked repo, make sure the base
is your own fork's `main` branch as opposed to the default PR behaviour which chooses the original
fork's `main` branch as the base of your PR.

From your repo's Github URL, create and merge your PR. Go to the `Actions` tab, you should see that
the workflow `Deploy Documentation` is running. Click on it to observe the details of each step.
Once all steps have completed, you'll see that a new workflow `pages-build-deployment` has kicked
off. This workflow is triggered by `Github` when changes are pushed to `gh-pages`.

When this workflow is completed, refresh your Github Pages URL. You should see the new change on
your site.

With this new workflow in place, from now on every newly merged Pull Request into your project is
going to trigger a new deployment of your documentation!

{: .hint }
🙌 You have now reached the
[`8-automating-updates`](https://github.com/aelsayed95/the-office/tree/8-automating-updates) part
of the tutorial. If not, check-out that branch and
[8-gh-pages](https://github.com/aelsayed95/the-office/tree/8-gh-pages) branch for `gh-pages` and
continue from there.

<br />
[Previous: Hosting on GitHub Pages](./hosting-on-github-pages.md){: .btn .float-left .mb-lg-4}
[Next: Versioning](./versioning.md){: .btn .btn-purple .float-right .mb-lg-4}
<br />
