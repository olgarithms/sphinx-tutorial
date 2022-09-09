---
layout: default
title: 1. Environment setup
nav_order: 1
---

# Environment setup

---

The prerequisites for this workshop are having `git` and `python` installed
([`python3.10`](https://www.python.org/downloads/) is recommended), as well as a
[`GitHub`](https://github.com/) account

You can choose to work on your own environment using any IDE, although there will be some extra
tips provided for a local workflow using [`Visual Studio Code`](https://code.visualstudio.com/).

Our first step will be **forking** and **cloning** the
[example project](https://github.com/aelsayed95/the-office), so run

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
