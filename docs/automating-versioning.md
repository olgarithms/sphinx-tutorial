---
layout: default
title: Automating versioning
nav_order: 11
---

# Automating versioning

---

We are now ready to integrate `sphinx-multiversion` into our automated deployment workflow. We'll
modify our existing workflow so that it uses `sphinx-multiversion`.

Open up `.gitub/workflows/docs.yaml`. First, modify the step `Install dependencies` to install
`sphinx_multiversion`:

```yaml
- name: Install dependencies
  run: |
    pip install sphinx sphinx_multiversion
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

Your file `.gitub/workflows/docs.yaml` should now look like this:

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
          pip install sphinx sphinx_multiversion
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
# from the repo's root
git add .gitub/workflows/docs.yaml
git commit -m "use sphinx-multiversion"
git push origin main
```

To test this, create a new branch `update_docs2`, make a small change to `docs/index.rst`. Commit
and push the change to your new branch. Create a PR. When you merge the PR, from the `Pages` tab on
your repo's Github URL, you should see that the `Deploy Documentation` workflow has kicked off.
Inspect the job details to verify that `sphinx-multiversion` has run. Refresh your Github Pages
URL. You should see your update has taken place on the `main` version as well as the `update_docs2`
version of your docs.

## Incrementing Documentation Versions Easily

To make incrementing the documentation version easier, we'll modify our workflow so that if the
pull request uses the label `docs-version++`, it will create a new github tag that's an increment
on the previous tag. Otherwise, it will simply rebuild and deploy documentation updates to `main`.

Let's create a simple shell script `create_tag.sh`, that will increment the github tag:

```sh
# get current tag
CURTAG=`git describe --abbrev=0 --tags`;
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

Next, in `.github/workflow/docs.yaml` and add a new step, following the `Git config` step that will
execute the script `create_tag.sh`, if the label `docs-version++` is used:

```yaml
- name: Create new tag
  if: contains(github.event.pull_request.labels.*.name, 'docs-version++')
  run: |
    ./create_tag.sh
```

We now have all the components needed to automate versioning your documentation. Let's commit the
change.

```
git checkout main
git add create_tag.sh
git add .github/workflow/docs.yaml
git commit -m "increment docs version easily"
git push origin main
```

Now, we are ready to test this. In a new branch, make a small addition to `index.rst`, commit, push
and create a PR. On the PR, add a label `docs-version++`. When you merge your PR, you should see
that `Deploy Documentation` Action kicked off. Verify that the step `Create new tag` has run
successfully as part of your workflow and has created the tag `v0.0.2`. When your workflow is done
running, and the subsequent `pages-build-deployment` has finished running, refresh your Pages URL.
You should now see the new tag `v0.0.2` is linked on the sidebar of your Pages site.

We are done! We recommend you take a look at the [Further reading](#further-reading) section if you
want to learn more about documentation.
