---
layout: default
title: 4. Documenting your API
nav_order: 4
---

# Documenting your API

---

Before generating our documentation with Sphinx we need to document our code.

Open the file `office.py`. We are going to add
[Python docstrings](https://peps.python.org/pep-0257/) to the class `TheOffice`  In this tutorial we will follow the
[Google-style](https://google.github.io/styleguide/pyguide.html#38-comments-and-docstrings)
docstring. You can browse examples of this style
[here](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html).

{: .tip }
Sphinx docstrings are traditionally written in `reStructuredText`, but they
are much more dense and hard to read. The Google-style docstring is friendlier
to the eye while browsing the source code.

Locate the method `get_employee_sales()` and add a Google-style docstring to document it:

```py
def get_employee_sales(self, employee: Employee) -> int:
    """Method that returns the number of sales given an employee.

    Args:
        employee (Employee): The employee whose sales number we are interested in.

    Returns:
        int: The total number of sales the employee has made.
    """
```

{: .tip }
In VS Code, you can install the extension `autoDocstring` which will generate python
docstring prompts, so you only need to fill them in. PyCharm supports this as well.

Continue adding documentation for the rest of the methods and for the class itself. Repeat for
`employee.py`.

When you're done, commit the changes you made:

```sh
git add office.py employee.py
git commit -m "Added docstrings"
git push origin main
```

{: .hint }
🙌 You have now reached the
[`4-documenting-your-api`](https://github.com/aelsayed95/the-office/tree/4-documenting-your-api)
part of the tutorial. If not, check-out that branch and continue from there.

<br />
[Previous: reStructuredText basics](./restructuredtext-basics.md){: .btn .float-left}
[Next: Generating HTML docs from docstrings](./generating-docs.md){: .btn .btn-purple .float-right}
<br />
