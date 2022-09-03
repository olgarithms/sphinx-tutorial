---
layout: default
title: Documenting your API
nav_order: 5
---

# Documenting your API

---

One of the most powerful and popular `Sphinx` extensions is `sphinx.ext.autodoc`. It allows you to
generate easy to read API documentation and keep it in sync with your code. Let's learn how to use
it.

Open the file `office.py`. We are going to to create
[Python docstrings](https://peps.python.org/pep-0257/) in order to explain how to use the
interface. In this tutorial we will follow the [Google-style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html)
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
In VS Code, you can install the extension `autoDocstring` which will generate python docstrings for you, so you only need to fill them in. `PyCharm` supports this as well.

Continue adding documentation for the rest of the methods and for the class itself.
