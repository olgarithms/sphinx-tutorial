---
layout: default
title: 1. Environment setup
nav_order: 1
---

# Environment setup

---

## Prerequisites

The prerequisites for this workshop are having `git` and `python` installed
([`python3.10`](https://www.python.org/downloads/) is recommended), as well as having a
[GitHub](https://github.com/) account.

You can choose to work on your own environment using any IDE, although there will be some extra
tips provided for a local workflow using [Visual Studio Code](https://code.visualstudio.com/).

## Creating your project

Create a new, public repository on your personal GitHub organisation from our
[template project](https://github.com/aelsayed95/the-office), by clicking on the green button
"**Use this template**" near the top. Name your repo `the-office`, and check ✅ the option
"**Include all branches**".

Next, clone your repo locally:

```sh
git clone git@github.com:<your-org>/the-office.git
cd the-office
```

## Using a virtual environment

Next, we will be setting up a [virtual environment](https://docs.python.org/3/tutorial/venv.html),
where later we will be installing everything else we need for the workshop.

```sh
python3.10 -m venv venv
source venv/bin/activate
```

In order to exit your virtual environment at any point, you can run

```sh
deactivate
```

<br />
[Previous: Home](../index.md){: .btn .float-left}
[Next: Getting started with Sphinx](./getting-started-with-sphinx.md){: .btn .btn-purple .float-right}
<br />
