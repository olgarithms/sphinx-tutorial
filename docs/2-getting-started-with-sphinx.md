---
layout: default
title: 2. Getting started with Sphinx
nav_order: 2
---

# Getting started with Sphinx

---

Begin by installing Sphinx inside your virtual environment.

```sh
pip install sphinx
```

At the repo's top level create a `docs` directory. This is where our documentation source and
output files will live.

```sh
mkdir docs
```

## Starting with Sphinx

We are now ready to start using Sphinx. Begin by the following script:

```sh
sphinx-quickstart docs
```

It will ask you a few questions to setup your documentation and we are going to go with the default
options for this tutorial:

```
> Separate source and build directories (y/n) [n]:  <Press Enter or n>
> Project name:                                     Sphinxy
> Author name(s):                                   <Enter your name>
> Project release []:                               <Press Enter>
> Project language [en]:                            <Press Enter>
```

Once the script has run, you should find the following in your `docs` directory:

- `conf.py`: configuration file which allows you to set Sphinx's behaviour (see configuration
  options on the [docs](https://www.sphinx-doc.org/en/master/usage/configuration.html))
- `index.rst`: the source file for your documentation's home page
- `Makefile`/`make.bat`: contain convenient `make` targets for generating documentation
- `_build/`: directory where your output documentation files will live
- `_static/:`: directory where all your static files, e.g. images
- `_templates/`: directory where you can override Sphinx template files to customise the look of
  your documentation

Take a moment to look at the content of each of these files. We are now ready to build our
documentation.

```sh
cd docs
make html
```

{: .tip }
If you are using a Windows machine and `make` is not available, you can use the `make.bat` script
instead of the `make` command: `.\make.bat html`.

Executing `make html` runs `sphinx-build`, pointing to the source and build directories to use and
instructs Sphinx to build `html` pages out of your `rst` files.

{: .tip }
You can run `make` without any arguments or `make help` to see what other targets are available.

## Previewing your docs

Now you preview your docs by manually opening the generated `_build/html/index.html` in your
preferred browser.

Alternatively, we can run our own python server in the `_build/html` directory, so that we can also
move around the pages. From the `docs` directory run the command:

```py
# from the `docs` dir
python -m http.server --directory _build/html
```

which opens a web server at port `8000`. Access your docs by visiting
[`http://localhost:8000`](http://localhost:8000) on your browser.

As you go on with the workshop, you can leave your server running and refreshing the webpage will
show you the latest updates.

You may have noticed that the `Module Index` link doesn't work. This is because we have yet to document our API and link the module doc files to our sphinx website. You'll learn how to do that in the next few sections.

{: .hint }
🙌 You have now reached the
[`2-getting-started-with-sphinx`](https://github.com/aelsayed95/sphinxy/tree/2-getting-started-with-sphinx)
part of the tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: Environment setup](./1-environment-setup.md){: .btn .float-left}
[Next: reStructuredText basics](./3-restructuredtext-basics.md){: .btn .btn-purple .float-right}
<br />
