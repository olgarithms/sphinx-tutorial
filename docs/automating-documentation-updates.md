---
layout: default
title: 8. Automating updates
nav_order: 8
---

# Automating documentation updates

Github Actions allow Github users to automatically execute workflows. We can use this to automate
building and deploying new versions of our documentation. First, let's build a simple Github
Actions workflow to understand how it works. Let's create a workflow that prints "Hello, World!"
when a commit is pushed to `main`:

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
      - name: echo
        run: echo "Hello, World!"
```

This file defines a workflow named `Hello World`, which is triggered by a push to the `main`
branch. It runs a single job `hello`, which has a single step `echo` that prints `Hello, Wolrd!` to
stdout. To test it out, commit and push this file to `main`.

Now, navigate to the `Actions` tab on your repo's Github URL. You should see that a `workflow` with
the name `Hello World` has run. Click on the workflow, and click on the latest commit. Click on the
job `hello`. Now expand the step `echo` to see its details. You should see the message
`Hello, World!` printed to stdout.

You now know how to build a simple workflow. Let's now write a workflow that would build and deploy
our documentation when a PR is merged. Create a new file `.github/workflows/docs.yaml`:

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

Finally, we build our Sphinx documentation using `sphinx-build` and push the output `_build` folder
to `gh-pages`:

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
          pip install sphinx
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

```markdown
# Welcome to The Office's documentation!

Explore The Office documentation.
```

Commit your change to a new branch and create a PR:

```sh
git checkout -b update_docs
git add docs/index.rst
git commit -m "update docs"
git push origin update_docs
```

From your repo's Github URL, create and merge your PR. Go to the `Actions` tab, you should see that
the workflow `Deploy Documentation` is running. Click on it to observe the details of each step.
Once all steps have completed, you'll see that a new workflow `pages-build-deployment` has kicked
off. This workflow is triggered by Github when changes are pushed to `gh-pages`. Once it completes,
refresh your Github Pages URL. You should see the new change on your site.

<br />
[Previous: Hosting on GitHub Pages](./hosting-on-github-pages.md){: .btn .float-left .mb-lg-4}
[Next: Versioning](./versioning.md){: .btn .btn-purple .float-right .mb-lg-4}
<br />
