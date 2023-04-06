---
layout: default
title: 4. Documenting your API
nav_order: 4
---

# Documenting your API

---

Before generating our documentation with Sphinx we need to document our code.

Open the file `sphinx.py`. We are going to add
[Python docstrings](https://peps.python.org/pep-0257/) to the class `Sphinx`.  In this tutorial we will follow the
[Google-style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
docstring. You can browse examples of this style
[here](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html).

{: .tip }
Sphinx docstrings are traditionally written in `reStructuredText`, but they
are much more dense and hard to read. The Google-style docstring is friendlier
to the eye while browsing the source code.

Locate the method `check_riddle_answer()` and add a Google-style docstring to document it:

```py
def check_riddle_answer(self, answer: str, return_hint: bool = False) -> str:
        """Evaluates the answer given for he riddle.

        Args:
            answer (str): The given answer to the riddle.
            return_hint (bool, optional): Controls whether a hint for the riddle should be returned.
                Defaults to False.

        Raises:
            IncorrectAnswer: Exception for incorrect answer.

        Returns:
            str: The result of the evaluation of the answer.
        """
```

{: .tip }
In VS Code, you can install the extension `autoDocstring` which will generate python
docstring prompts, so you only need to fill them in. PyCharm supports this as well.

Continue adding documentation for the rest of the methods and for the class itself. Repeat for
`riddle.py`.

When you're done, commit the changes you made:

```sh
# from the project root
git add sphinx.py riddle.py
git commit -m "Added docstrings"
git push origin main
```

{: .hint }
🙌 You have now reached the
[`4-documenting-your-api`](https://github.com/aelsayed95/sphinxy/tree/4-documenting-your-api)
part of the tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: reStructuredText basics](./restructuredtext-basics.md){: .btn .float-left}
[Next: Generating HTML docs from docstrings](./5-generating-docs.md){: .btn .btn-purple .float-right}
<br />
