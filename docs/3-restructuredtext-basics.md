---
layout: default
title: 3. reStructuredText basics
nav_order: 3
---

# `reStructuredText` basics

---

[`reStructuredText`](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) is
the default plaintext markup language used by Sphinx.

## The `index.rst` file

There are many tutorials out there explaining the syntax of `.rst` files that you can try a
separate time, so we will focus now on what exists in our `index.rst` file.

### Heading

```
Welcome to The Office's documentation!
======================================
```

This is a heading containing the project name, as generated by Sphinx. In order to specify a new
section, create a heading by underlining the text with any punctuation character as long as the
text. Using the same character will create headings of the same level.

### Table of contents tree

```
.. toctree::
   :maxdepth: 2
   :caption: Contents:
```

- The `toctree` (table of contents tree) adds structure to your docs.
- The `maxdepth` parameter specifies how many levels of headers you would like Sphinx to create
  in its output.
- The `caption` parameter gives option to provide a `toctree` caption, i.e. the _title_ of the
  `toctree`; in this case "Contents".

## Creating a section

Now let's add some more content by modifying the file `index.rst`. We are going to add a new
section that has some more information and a picture of the workspace in the office.

Create a new section **above** the "Indices and tables" one:

```
Workspace
---------
```

### Image

And now let's add a picture to our new section. You can use any image you like; we chose one we
found online and downloaded from [unsplash](https://unsplash.com). Save your image in the
`docs/_static` directory and type under your header:

```
.. image:: _static/workspace.jpg
```

Now re-run `make html` to see your image rendered on the site.

{: .hint }
🙌 You have now reached the
[`3-rst-text-basics`](https://github.com/aelsayed95/the-office/tree/3-rst-text-basics) part of the
tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: Getting started with Sphinx](./2-getting-started-with-sphinx.md){: .btn .float-left}
[Next: Documenting your API](./4-documenting-your-api.md){: .btn .btn-purple .float-right}
<br />