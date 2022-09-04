---
layout: default
title: Getting started with sphinx
nav_order: 3
---

# Getting started with `sphinx`

---

Begin by installing `sphinx` inside your virtual environment.

```sh
python3.10 -m pip install sphinx
```

At the repo's top level create a `docs` directory. This is where our documentation source and
output files will live.

```sh
mkdir docs
```

We are now ready to start using `Sphinx`. Begin by the following script:

```sh
sphinx-quickstart docs
```

It will ask you a few questions to setup your documentation. We'll go with the default options for
this tutorial. Once the script has run, you should find the following in your `docs` directory:

- `conf.py`: configuration file which allows you to set `Sphinx`'s behaviour (see configuration
  options on the [docs](https://www.sphinx-doc.org/en/master/usage/configuration.html))
- `index.rst`: the source for your documentation's home page
- `Makefile`/`make.bat`: contain convenient `make` targets for generating documentation
- `_build/`: directory where your output documentation files will live
- `_static:`: directory where all your static files, e.g. images, go to
- `_templates`: directory where you can override `sphinx` template files to customize the look of
  your documentation

Take a moment to look at the content of each of these files. We are now ready to build our
documentation.

```sh
cd docs
make html
```

As you might have seen in the `Makefile`, this runs `sphinx-build`, pointing to the source and
build directories to use and instructs `sphinx` to build `html` documentation out of your `rst`
files.

Now you preview your docs by manually opening the generated `_build/html/index.html` in your
preferred browser.

{: .note }
In VS Code, you can get the `HTML Preview` extension to preview the output `html` files.

Alternatively, we can run our own python server in the `_build/html` directory, so that we can also
move around the pages. Run the command

```py
python3.10 -m http.server
```

which opens a web server at port `8000`. Access your docs by visiting `http://localhost:8000` on
your browser.

As you go on with the workshop, you can leave your server running and refreshing the webpage will
show you the latest updates.
