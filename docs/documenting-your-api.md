---
layout: default
title: 4. Documenting your API
nav_order: 4
---

# Documenting your API

---

One of the most powerful and popular `Sphinx` extensions is `sphinx.ext.autodoc`. It allows you to
generate easy to read API documentation and keep it in sync with your code. Let's learn how to use
it.

Open the file `office.py`. We are going to add
[Python docstrings](https://peps.python.org/pep-0257/) to the class `TheOffice` to document how to use the
API. In this tutorial we will follow the
[Google-style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
docstring. There are other styles you can explore, outside the scope of this tutorial.

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

{: .note }
In VS Code, you can install the extension `autoDocstring` which will generate python
docstring prompts, so you only need to fill them in. `PyCharm` supports this as well.

Continue adding documentation for the rest of the methods and for the class itself. Repeat for `employee.py`.

<br />
[Previous: reStructuredText basics](./restructuredtext-basics.md)){: .btn .float-left .mb-lg-4}
[Next: Generating HTML docs from docstrings](./generating-docs.md){: .btn .btn-purple .float-right .mb-lg-4}
<br />
