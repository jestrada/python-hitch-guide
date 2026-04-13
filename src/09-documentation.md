# Documentation

![image](https://docs.python-guide.org/_static/photos/35620636012_f66aa88f93_k_d.jpg)

Readability is a primary focus for Python developers, in both project and code documentation. Following some simple best practices can save both you and others a lot of time.

## Project Documentation

A **README** file at the root directory should give general information to both users and maintainers. It should be written in Markdown or reStructuredText and contain:
- A few lines explaining the purpose of the project
- The URL of the main source
- Basic credit and license information
- Installation instructions

An **INSTALL** section is less necessary with modern Python. Installation is often reduced to one command:

```bash
$ uv add mypackage
# or
$ uv pip install mypackage
```

A **LICENSE** file should *always* be present and specify the license under which the software is made available.

A **CHANGELOG** file or section should compile a short overview of changes for the latest versions.

## Project Publication

Depending on the project, your documentation might include:

- An **introduction** — a very short overview with one or two simplified use cases. The thirty-second pitch.
- A **tutorial** — primary use cases in detail, step-by-step.
- An **API reference** — typically generated from docstrings. Lists all public interfaces, parameters, and return values.
- **Developer documentation** — for potential contributors. Code conventions and design strategy.

### Sphinx

[Sphinx](https://www.sphinx-doc.org) is the most popular Python documentation tool. It converts reStructuredText into HTML, LaTeX, man pages, and more.

Free hosting is available at [Read the Docs](https://readthedocs.org). Configure it with commit hooks for automatic rebuilds.

### MkDocs

[MkDocs](https://www.mkdocs.org/) is a fast, simple static site generator for building project documentation with Markdown. It's a popular alternative to Sphinx, especially for projects already using Markdown.

### reStructuredText

Most Python documentation is written with [reStructuredText](https://docutils.sourceforge.net/rst.html). It's like Markdown but with more built-in extensions.

See the [reStructuredText Primer](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) for syntax help.

## Code Documentation Advice

Comments clarify the code and are added to make the code easier to understand. In Python, comments begin with `#`.

### Docstrings

In Python, *docstrings* describe modules, classes, and functions:

```python
def square_and_rooter(x):
    """Return the square root of self times self."""
    ...
```

Follow [PEP 8](https://peps.python.org/pep-0008/) for comments and [PEP 257](https://peps.python.org/pep-0257/) for docstring conventions.

### Commenting Sections of Code

*Do not use triple-quote strings to comment code*. Line-oriented tools like `grep` won't be aware the code is inactive. Use `#` at the proper indentation level. Your editor has a comment/uncomment toggle — learn it.

### Docstrings and Magic

Tools like [Sphinx](https://www.sphinx-doc.org) parse docstrings as reStructuredText and render them as HTML. This makes it easy to embed example code.

[Doctest](https://docs.python.org/3/library/doctest.html) reads embedded docstrings that look like Python REPL sessions and runs them, verifying the output matches:

```python
def my_function(a, b):
    """
    >>> my_function(2, 3)
    6
    >>> my_function('a', 3)
    'aaa'
    """
    return a * b
```

### Writing Docstrings

For simple functions, a one-line docstring is appropriate:

```python
def add(a, b):
    """Add two numbers and return the result."""
    return a + b
```

For more complex code, the [NumPy style](https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_numpy.html) is popular:

```python
def random_number_generator(arg1, arg2):
    """
    Summary line.

    Extended description of function.

    Parameters
    ----------
    arg1 : int
        Description of arg1
    arg2 : str
        Description of arg2

    Returns
    -------
    int
        Description of return value
    """
    return 42
```

The [sphinx.ext.napoleon](https://sphinxcontrib-napoleon.readthedocs.io/) plugin lets Sphinx parse this style.

Google style is another popular option:

```python
def function_with_types(arg1: str, arg2: int) -> bool:
    """Summary line.

    Args:
        arg1: Description of arg1.
        arg2: Description of arg2.

    Returns:
        Description of return value.

    Raises:
        ValueError: If arg2 is negative.
    """
    ...
```

At the end of the day, it doesn't matter which style you use — as long as it's correct, understandable, and gets the relevant points across.

## Other Tools

- [Pycco](https://pycco-docs.github.io/pycco/) — literate-programming-style documentation generator
- [MkDocs](https://www.mkdocs.org/) — fast, simple static site generator for Markdown documentation
- [Jupyter Book](https://jupyterbook.org/) — build books and docs from Markdown and notebooks
