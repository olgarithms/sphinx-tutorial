---
layout: default
title: 10. Automating versioning
nav_order: 10
---

# Automating versioning

---

We are now ready to integrate `sphinx-multiversion` into our automated deployment workflow. We'll
modify our existing workflow so that it uses `sphinx-multiversion`.

Open up `.github/workflows/docs.yaml`. First, modify the step `Install dependencies` to install
`sphinx_multiversion`:

```yaml
- name: Install dependencies
  run: |
    pip install sphinx furo sphinx_multiversion
```

Next, modify the step `Sphinx build` to use the `sphinx_multiversion` command to build your
documentation:

```yaml
- name: Sphinx build
  run: |
    git worktree add docs/_build/html gh-pages
    cd docs
    sphinx-multiversion . ./_build/html
```

Your file `.github/workflows/docs.yaml` should now look like this:

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
          python-version: "3.11"
      - name: Install dependencies
        run: |
          pip install sphinx furo sphinx_multiversion
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
          sphinx-multiversion . ./_build/html
      - name: Deploy docs
        run: |
          cd docs/_build/html
          git add --all
          git commit -m "deploy documentation updates"
          git push origin gh-pages
```

Now, commit your updated workflow to `main`:

```sh
# from the project root
git add .github/workflows/docs.yaml
git commit -m "Use sphinx-multiversion in docs action"
git push origin main
```

To test this, create a new branch `update_docs2`, make a small change to `docs/index.rst`. Commit
and push the change to your new branch. Create a PR. When you merge the PR, from the `Pages` tab on
your repo's GitHub URL, you should see that the `Deploy Documentation` workflow has kicked off.
Inspect the job details to verify that `sphinx-multiversion` has run. Refresh your GitHub Pages
URL. You should see your update has taken place on the `main` version as well as the `update_docs2`
version of your docs. Make sure to pull the latest changes to your local `main`.

## Incrementing Documentation Versions Easily

To make incrementing the documentation version easier, we'll modify our workflow so that if the
pull request uses the label `version++`, it will create a new git tag that's an increment on the
previous tag. Otherwise, it will simply rebuild and deploy documentation updates to `main`.

Let's create a simple shell script `create_tag.sh` in the root of our project, that will increment
the git tag:

```sh
# get current tag
CURTAG=`git tag`;
CURTAG="${CURTAG/v/}"
echo "Current version is ${CURTAG}"

# parse major, minor, patch
IFS='.' read -a vers <<< "$CURTAG"
MAJ=${vers[0]:-0}
MIN=${vers[1]:-0}
PATCH=${vers[2]:-0}

# create new tag with patch++
((PATCH+=1))
NEWTAG="v$MAJ.$MIN.${PATCH}"
echo "Next version is $NEWTAG"
git tag -a $NEWTAG -m $NEWTAG
git push origin $NEWTAG
```

Next, add execute permissions to the shell file:

```sh
chmod ugo+x create_tag.sh
```

Next, in `.github/workflow/docs.yaml` and add a new step, following the `Git config` step that will
execute the script `create_tag.sh`, if the label `version++` is used:

```yaml
- name: Create new tag
  if: contains(github.event.pull_request.labels.*.name, 'version++')
  run: |
    ./create_tag.sh
```

We now have all the components needed to automate versioning your documentation. Let's commit the
change.

```sh
git checkout main
git add create_tag.sh
git add .github/workflows/docs.yaml
git commit -m "increment docs version easily"
git push origin main
```

Now, we are ready to test this. In a new branch, make a small addition to `index.rst`, commit, push
and create a PR. On the PR, add a label `version++`. When you merge your PR, you should see that
`Deploy Documentation` Action kicked off. Verify that the step `Create new tag` has run
successfully as part of your workflow and has created the tag `v0.0.2`. When your workflow is done
running, and the subsequent `pages-build-deployment` has finished running, refresh your Pages URL.
You should now see the new tag `v0.0.2` is linked on the sidebar of your Pages site.

Finally, you'll notice that not all branches appear on your website when `sphinx-multiversion` ran
remotely. If you want to change this, you can configure this behaviour by adding the following to
`conf.py`:

```py
# -- Sphinx Multiversion --------------------------------------------------
# https://holzhaus.github.io/sphinx-multiversion/master/configuration.html#
smv_tag_whitelist = r'^v\d+\.\d+\.\d+$'
smv_branch_whitelist = r'^.*$'
smv_remote_whitelist = r'^.*$'
```

{: .hint }
🙌 You have now reached the
[`10-automating-versioning`](https://github.com/aelsayed95/the-office/tree/10-automating-versioning)
part of the tutorial. If not, check-out that branch and
[`10-gh-pages`](https://github.com/aelsayed95/the-office/tree/10-gh-pages) branch for `gh-pages`
and continue from there.

We are done! We recommend you take a look at the [Further reading](#further-reading) section if you
want to learn more about documentation generation.

<br />
[Previous: Versioning](./9-versioning.md){: .btn .float-left}
[Next: Glossary](./11-glossary.md){: .btn .btn-purple .float-right}
<br />
