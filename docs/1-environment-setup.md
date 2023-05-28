---
layout: default
title: 1. Environment setup
nav_order: 1
---

# Environment setup

---

## Prerequisites

The prerequisites for this workshop are having `git` and `python` installed
([`python3.11`](https://www.python.org/downloads/) is recommended), as well as having a
[GitHub](https://github.com/) account.

The tutorial focuses on UNIX-based systems, but we will providing some tips on how to run the
commands on others.

You can choose to work on your own environment using any IDE, although there will be some extra
tips provided for a local workflow using [Visual Studio Code](https://code.visualstudio.com/).

## Creating your project

### Using [GitHub Codespaces](https://github.com/features/codespaces) (Recommended)

1. To get started, create a new project using the example project Sphinxy as
   [a template](https://codespaces.new/aelsayed95/sphinxy) on GitHub Codespaces. This will open up
   a copy of Sphinxy on codespaces for you.

2. From the source control tab on the left panel of Codespaces, select "Publish Branch".

3. You'll be prompted to name your new remote repository, select the `public` option and name your
   repo `sphinxy`.

### On your computer

1. Create a new, public repository on your personal GitHub organisation from our
   [template project](https://github.com/aelsayed95/sphinxy), by clicking on the
   "[Use this template => Create a new repository](https://github.com/aelsayed95/sphinxy/generate)"
   button.

2. Name your repo `sphinxy`.

3. Clone your new repo locally:

   ```sh
   git clone git@github.com:<your-org>/sphinxy.git
   cd sphinxy
   ```

## Using a virtual environment

Next, we will be setting up a [virtual environment](https://docs.python.org/3/tutorial/venv.html),
where later we will be installing everything else we need for the workshop.

```sh
python3.11 -m venv venv
source venv/bin/activate
```

Finally, we'll install our package in edit mode. This will be needed later by some Sphinx
extensions:

```sh
pip install -e .
```

{: .tip }
If you are using a different OS and/or shell the command to activate your virtual
environment might differ, so check the _Platform | Shell | Command_ table in the
[docs](https://docs.python.org/3/library/venv.html#creating-virtual-environments).

In order to exit your virtual environment at any point, you can run:

```sh
deactivate
```

<br />
[Previous: Home](../index.md){: .btn .float-left}
[Next: Getting started with Sphinx](./2-getting-started-with-sphinx.md){: .btn .btn-purple .float-right}
<br />
