---
layout: default
title: 7. Hosting on GitHub Pages
nav_order: 7
---

# Hosting on GitHub Pages

---

## Creating a `gh-pages` branch

To host your docs on GitHub Pages, we need to enable GitHub Pages for our repo. To do that, we need
to create a branch that only contains `html` files. We'll name this branch `gh-pages`, as GitHub
looks for this name and automatically starts hosting your documentation on `pages`.

Let's create `gh-pages` as an
[orphan branch](https://git-scm.com/docs/git-checkout#Documentation/git-checkout.txt---orphanltnew-branchgt),
which means there is no previous history and commits:

```sh
# from the project root
cd ./docs/_build/html
git checkout --orphan gh-pages      # create the orphan branch
git reset --hard                    # overwrite all changes in the directory
git commit --allow-empty -m "Init"  # create a commit without any changes
git push origin gh-pages
```

In your remote repository, you should now see a new empty `gh-pages` branch. You will notice that
under `Settings > Pages`, GitHub has enabled `Pages` and started hosting your site at a URL like
`https://<username>.github.io/sphinxy/`. Of course, if you visit the URL, you get a 404 Error
because there are no files yet on the `gh-pages` branch.

Next, we locally mount the orphan` gh-pages` branch as a subdirectory using
[`git worktree`](https://git-scm.com/docs/git-worktree).

```sh
# from the docs/_build/html dir
cd ..
git checkout main
rm -rf html  # make sure you don't have an existing target directory
git worktree add html gh-pages
```

To check that you set up branches correctly, you should see that `git branch` shows `gh-pages`
under the `html` subdirectory, but see the `main` branch as usual outside of `html`:

```sh
# from the project root
$ git branch
    gh-pages
  * main

$ cd ./docs/_build/html

$ git branch
  * gh-pages
    main
```

Our branch `gh-pages` is completely empty. But GitHub will be looking for a top-level `index.html`.
So from the `./docs/_build/html` directory let's create a simple one:

```sh
# from the docs/_build/html dir
echo '<h1>Hello, World!</h1>' > index.html
git add index.html
git commit -m 'hello world landing page'
git push origin gh-pages
```

Go to the `Actions` tab of your repo's GitHub URL. You should see that a `pages-build-deployment`
workflow has kicked off. This is a workflow that rebuilds your GitHub Pages site and is triggered
by our push to `gh-pages`. Once it's completed, refresh the `Pages` URL where your site is hosted.
You should see the heading `Hello, World!`.

### Cleaning the HTML files

In the previous section, we mounted the `gh-pages` branch at `./docs/_build/html`. But you may
occasionally want to clean your documentation build output. Running `make clean` inside your `docs`
directory will remove everything under `_build`.

This means that, if you want to be able to push more updates to `gh-pages`, you will need to
recreate the `git worktree` every time you `make clean`:

`git worktree add -f html gh-pages`.

To make this easier, add the following to `docs/Makefile`:

```make
clean:
	@$(SPHINXBUILD) -M clean "$(SOURCEDIR)" "$(BUILDDIR)" $(SPHINXOPTS) $(O);
	cd _build; git worktree add -f html gh-pages
```

With these changes, `make clean` will clean the `_build/` directory, and populate the `html`
directory with the current contents on the `gh-pages` branch. So it will effectively clean your
local documentation changes.

Commit your updated `Makefile`:

```sh
# from the project root
git add docs/Makefile
git commit -m "Add updated clean target in Makefile"
git push origin main
```

## Disabling Jekyll

Before we can deploy our API docs on GitHub Pages, we need to tell GitHub not to use `Jekyll`: a
static site generator tool that GitHub Pages uses by default. We could turn off its usage by
committing an empty `.nojekyll` file our `_build/html` folder.

Instead of committing a `.nojekyll` file, you can add the extension `sphinx.ext.githubpages` to
your `conf.py`. It automatically adds the `.nojekyll` file on `make html`.

```py
extensions = [
    "sphinx.ext.autodoc",
    "sphinx.ext.napoleon",
    "sphinx.ext.githubpages",
]
```

Let's also commit the change in `conf.py`:

```sh
# from the project root
git add docs/conf.py
git commit -m "Add githubpages extension"
git push origin main
```

## Hosting the documentation

Next, we want to add the Sphinxy documentation to `gh-pages`. Let's generate our documentation
again and check in our output files.

```sh
cd docs
make clean  # clean and re-create the gh-pages worktree
make html
cd _build/html
git branch  # verify you're in gh-pages branch
git add -A
git commit -m "First documentation commit"
git push origin gh-pages
```

Go back to your repo's `Action` tab. Once `pages-build-deployment` workflow has completed, hit
refresh on your Pages URL.

🎉 **Congratulations!** 🎉

Now your documentation is now live on GitHub Pages! You should be able to navigate around the
website and share the URL with the world!

{: .hint }
🙌 You have now reached the
[`07-gh-pages`](https://github.com/aelsayed95/sphinxy/tree/07-gh-pages) part of
the tutorial. If not, check-out that branch and
[`7-gh-pages`](https://github.com/aelsayed95/sphinxy/tree/7-gh-pages) branch for `gh-pages` and
continue from there.

<br />
[Previous: Selecting a theme](./6-selecting-a-theme.md){: .btn .float-left}
[Next: Automating documentation updates](./8-automating-documentation-updates.md){: .btn .btn-purple .float-right}
<br />
