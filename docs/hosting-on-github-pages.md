---
layout: default
title: 7. Hosting on Github pages
nav_order: 7
---

# Hosting on Github pages

---

To host your docs on GitHub Pages, we need to start by enabling GitHub Pages for our repo. To do
that, we need to create a branch that only contains documentation files. We'll name this branch
`gh-pages`, as Github looks for this name and automatically starts hosting your documentation on
`pages`.

We'll start by creating `gh-pages` as an empty orphan branch:

```sh
cd ./docs/_build/html
git checkout --orphan gh-pages
git reset --hard
git commit --allow-empty -m "Init"
git push origin gh-pages
```

In your remote repository, you should now see a new empty `gh-pages` branch. Next, we locally mount
the branch as a subdirectory using `git worktree`.

```sh
cd ./docs/_build/
git checkout main
rm -rf html # make sure you don't have an existing target directory
git worktree add html gh-pages
```

To check that you set up branches correctly, you should see `git branch` shows `gh-pages` under the
`html` subdirectory, but see the `main` branch as usual outside of `html`:

```sh
# from the project root
git branch
  gh-pages
* main

cd ./docs/_build/html
git branch
* gh-pages
  main
```

Our branch `gh-pages` is now completely empty. But `Github` will be looking for a top-level
`index.html`. So let's create a simple one:

```sh
echo "<h1>hello world</h1>" > index.html
git add index.html
git commit -m 'initial commit of gh-pages branch'
git push origin gh-pages
```

{: .tip }
You can use the command `make clean` inside your `docs` directory to remove everything under `_build`.

This also means that every time you `make clean`, you need to recreate the git worktree: `git worktree add -f html gh-pages`. To make this easier, add the following to `docs/Makefile`:

```make
clean:
	@$(SPHINXBUILD) -M clean "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O);
	cd _build; git worktree add -f html gh-pages
```

Before we can preview this on `Github pages`, we need to tell Github not to use `Jekyll`: a static
site generator tool that GitHub Pages uses by default. We can turn off its usage by committing a
`.nojekyll` file our `_build/html` folder.

```sh
cd _build/html
touch .nojekyll
git add .
git commit -m "Don't use Jekyll"
git push origin gh-pages
```

{: .tip }
Alternatively, we can add the extension `sphinx.ext.githubpages` to our `conf.py`. It automatically adds the `.nojekyll` file for us on `make html`.

```py
# Add any Sphinx extension module names here, as strings. They can be
# extensions coming with Sphinx (named 'sphinx.ext.*') or your custom
# ones.
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.githubpages"
]
```

It's time to preview our hosted site. Navigate to your repo's `Settings` tab, under `Options`,
scroll down to the `GitHub Pages` section. You should see that Github recognized the presence of
the new `gh-pages` branch and gives you a link where you can view your hosted documentation. Click
on the link, you should be able to see the title `hello world` that we put in our `index.html`.

Next, we want to add The Office documentation to `gh-pages`. Let's generate our documentation again
and check in our output files.

```sh
cd docs/
make clean # cleans and create the gh-pages worktree
make html
cd _build/html
git branch # verify you're in gh-pages branch
git add -A
git commit -m "First documentation commit"
git push origin gh-pages
```

Go back to your Github Pages documentation URL and hit refresh.

🎉 **Congratulations!** 🎉

Now your documentation is now live on Github Pages!
You should be able to navigate around the website and share the URL with the world.

<br />
[Previous: Selecting a theme](./selecting-a-theme.md){: .btn .float-left .mb-lg-4}
[Next: Automating documentation updates](./automating-documentation-updates.md){: .btn .btn-purple .float-right .mb-lg-4}
<br />
