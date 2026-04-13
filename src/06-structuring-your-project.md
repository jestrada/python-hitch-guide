# Structuring Your Project

![image](https://docs.python-guide.org/_static/photos/33907151224_0574e7dfc2_k_d.jpg)

By "structure" we mean the decisions you make concerning how your project best meets its objective. We need to consider how to best leverage Python's features to create clean, effective code. In practical terms, "structure" means making clean code whose logic and dependencies are clear as well as how the files and folders are organized in the filesystem.

Which functions should go into which modules? How does data flow through the project? What features and functions can be grouped together and isolated? By answering questions like these you can begin to plan, in a broad sense, what your finished product will look like.

In this section, we take a closer look at Python's modules and import systems as they are the central elements to enforcing structure in your project. We then discuss various perspectives on how to build code which can be extended and tested reliably.

## Structure of the Repository

### It's Important

Just as Code Style, API Design, and Automation are essential for a healthy development cycle, repository structure is a crucial part of your project's architecture.

When a potential user or contributor lands on your repository's page, they see a few things:

- Project Name
- Project Description
- Bunch O' Files

Only when they scroll below the fold will the user see your project's README.

If your repo is a massive dump of files or a nested mess of directories, they might look elsewhere before even reading your beautiful documentation.

> Dress for the job you want, not the job you have.

Of course, first impressions aren't everything. You and your colleagues will spend countless hours working with this repository, eventually becoming intimately familiar with every nook and cranny. The layout is important.

### Sample Repository (Modern)

Here's a modern Python project using the `src` layout and `pyproject.toml`:

```
README.md
LICENSE
pyproject.toml
uv.lock
src/
    sample/
        __init__.py
        core.py
        helpers.py
docs/
    conf.py
    index.rst
tests/
    test_basic.py
    test_advanced.py
```

Let's get into some specifics.

### pyproject.toml

The `pyproject.toml` file is the modern standard for Python project configuration (defined by [PEP 517](https://peps.python.org/pep-0517/), [PEP 518](https://peps.python.org/pep-0518/), and [PEP 621](https://peps.python.org/pep-0621/)). It replaces `setup.py`, `setup.cfg`, and `requirements.txt`.

A minimal `pyproject.toml`:

```toml
[project]
name = "sample"
version = "1.0.0"
description = "A sample project"
requires-python = ">=3.12"
dependencies = [
    "requests>=2.31",
]

[project.optional-dependencies]
dev = [
    "pytest>=8.0",
    "ruff>=0.4",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Use `uv` to manage this file:

```bash
$ uv init              # create a new project
$ uv add requests      # add a dependency
$ uv add --dev pytest   # add a dev dependency
$ uv sync              # install all dependencies
```

### The src Layout

Placing your package inside a `src/` directory is now considered best practice. This prevents accidental imports from the project root during development and ensures you're testing the *installed* package, not the source directly.

```
src/
    sample/
        __init__.py
        core.py
```

### License

Place the full license text and copyright claims in `./LICENSE`. If you aren't sure which license to use, check out [choosealicense.com](https://choosealicense.com).

You are free to publish code without a license, but this prevents many people from using or contributing to your code.

### Documentation

Documentation belongs in `./docs/`. There's little reason for it to exist elsewhere.

### Test Suite

*For advice on writing your tests, see [Testing Your Code](./10-testing.md).*

Tests belong in `./tests/`. Starting out, a small test suite might exist in a single file:

```
./test_sample.py
```

Once the suite grows:

```
tests/test_basic.py
tests/test_advanced.py
```

With the `src` layout and a properly configured `pyproject.toml`, tests can import your package directly — no path hacks needed:

```python
# tests/test_basic.py
from sample.core import something

def test_something():
    assert something() == "expected"
```

Install the package in development mode:

```bash
$ uv sync
# or
$ uv pip install -e .
```

### Makefile or Just

A `Makefile` (or [Justfile](https://github.com/casey/just)) is useful for defining common tasks:

```makefile
init:
    uv sync

test:
    uv run pytest tests

lint:
    uv run ruff check .
    uv run ruff format --check .

.PHONY: init test lint
```

### Regarding Django Applications

When starting a Django project, run the command from within your repository:

```bash
$ django-admin startproject samplesite .
```

Note the `.` — this avoids unnecessary nesting. The resulting structure:

```
README.md
manage.py
samplesite/settings.py
samplesite/wsgi.py
samplesite/sampleapp/models.py
```

## Structure of Code is Key

Thanks to the way imports and modules are handled in Python, it is relatively easy to structure a Python project. Easy, here, means that you do not have many constraints and that the module importing model is easy to grasp. Therefore, you are left with the pure architectural task of crafting the different parts of your project and their interactions.

Easy structuring means it's also easy to do it poorly. Some signs of a poorly structured project include:

- **Multiple and messy circular dependencies:** If the classes Table and Chair in `furn.py` need to import Carpenter from `workers.py`, and the class Carpenter also needs to import Table and Chair, you have a circular dependency requiring fragile hacks.
- **Hidden coupling:** Each change in Table's implementation breaks 20 tests in unrelated test cases because it breaks Carpenter's code.
- **Heavy usage of global state:** Instead of explicitly passing `(height, width, type, wood)`, Table and Carpenter rely on global variables that can be modified on the fly by different agents.
- **Spaghetti code:** Multiple pages of nested if clauses and for loops with lots of copy-pasted procedural code and no proper segmentation.
- **Ravioli code:** Hundreds of similar little pieces of logic without proper structure. If you can never remember whether to use `FurnitureTable`, `AssetTable`, or `Table` for your task, you might be swimming in ravioli code.

## Modules

Python modules are one of the main abstraction layers available and probably the most natural one. Abstraction layers allow separating code into parts holding related data and functionality.

For example, a layer of a project can handle interfacing with user actions, while another would handle low-level manipulation of data. The most natural way to separate these two layers is to regroup all interfacing functionality in one file, and all low-level operations in another file.

As soon as you use `import` statements, you use modules. These can be either built-in modules such as `os` and `sys`, third-party modules you've installed, or your project's internal modules.

To keep in line with the style guide, keep module names short, lowercase, and avoid using special symbols like the dot (.) or question mark (?). A file name like `my.spam.py` should be avoided — it will interfere with the way Python looks for modules.

If you like, you could name your module `my_spam.py`, but even the underscore should not be seen that often in module names. Using other characters (spaces or hyphens) in module names will prevent importing. Try to keep module names short so there is no need to separate words. And, most of all, don't namespace with underscores; use submodules instead.

```python
# OK
import library.plugin.foo
# not OK
import library.foo_plugin
```

The `import modu` statement will look for `modu.py` in the same directory as the caller, then search the Python path recursively. When found, the interpreter executes the module in an isolated scope. Then, the module's variables, functions, and classes are available through the module's namespace.

It is possible to use `from modu import *`, but this is generally considered bad practice. **Using `import *` makes the code harder to read and makes dependencies less compartmentalized.**

**Very bad**

```python
from modu import *
x = sqrt(4)  # Is sqrt part of modu? A builtin? Defined above?
```

**Better**

```python
from modu import sqrt
x = sqrt(4)  # sqrt may be part of modu, if not redefined in between
```

**Best**

```python
import modu
x = modu.sqrt(4)  # sqrt is visibly part of modu's namespace
```

## Packages

Python provides a straightforward packaging system, which is simply an extension of the module mechanism to a directory.

Any directory with an `__init__.py` file is considered a Python package. A file `modu.py` in the directory `pack/` is imported with `import pack.modu`.

Leaving an `__init__.py` file empty is considered normal and even good practice, if the package's modules and sub-packages do not need to share any code.

A convenient syntax is available for importing deeply nested packages: `import very.deep.module as mod`. This allows you to use `mod` in place of the verbose repetition of `very.deep.module`.

## Object-Oriented Programming

In Python, everything is an object. Functions, classes, strings, and even types are objects: they have a type, they can be passed as function arguments, and they may have methods and properties.

However, unlike Java, Python does not impose object-oriented programming as the main programming paradigm. It is perfectly viable for a Python project to not be object-oriented.

Moreover, the way Python handles modules and namespaces gives the developer a natural way to ensure encapsulation and separation of abstraction layers, both being the most common reasons to use object-orientation.

There are some reasons to avoid unnecessary object-orientation. Defining custom classes is useful when we want to glue some state and some functionality together. The problem comes from the "state" part of the equation.

In some architectures, typically web applications, multiple instances of Python processes are spawned as a response to simultaneous requests. Holding state in instantiated objects is prone to concurrency problems or race conditions.

This and other issues led to the idea that using stateless functions is a better programming paradigm for many cases. A function's implicit context is made up of any global variables or items in the persistence layer that are accessed from within the function. Side-effects are the changes that a function makes to its implicit context.

Carefully isolating functions with context and side-effects from functions with logic (called **pure functions**) allows the following benefits:

- Pure functions are deterministic: given a fixed input, the output will always be the same.
- Pure functions are much easier to change or replace if they need to be refactored or optimized.
- Pure functions are easier to test with unit tests: less need for complex context setup and data cleaning afterwards.
- Pure functions are easier to manipulate, decorate, and pass around.

## Decorators

The Python language provides a simple yet powerful syntax called "decorators". A decorator is a function or class that wraps (or decorates) a function or method. Because functions are first-class objects in Python, this can be done "manually", but using the `@decorator` syntax is clearer and thus preferred.

```python
def foo():
    # do something

def decorator(func):
    # manipulate func
    return func

foo = decorator(foo)  # Manually decorate

@decorator
def bar():
    # Do something
# bar() is decorated
```

This mechanism is useful for separating concerns and avoiding external unrelated logic "polluting" the core logic of the function or method. A good example is [memoization](https://en.wikipedia.org/wiki/Memoization) or caching: you want to store the results of an expensive function and use them directly instead of recomputing.

Python's standard library includes `@functools.cache` (and `@functools.lru_cache`) for this purpose:

```python
from functools import cache

@cache
def expensive_function(n):
    return compute_result(n)
```

## Context Managers

A context manager provides extra contextual information to an action using the `with` statement. The most well-known example:

```python
with open('file.txt') as f:
    contents = f.read()
```

This ensures `f.close()` is called. There are two ways to create your own:

**Using a class:**

```python
class CustomOpen:
    def __init__(self, filename):
        self.file = open(filename)

    def __enter__(self):
        return self.file

    def __exit__(self, ctx_type, ctx_value, ctx_traceback):
        self.file.close()

with CustomOpen('file') as f:
    contents = f.read()
```

**Using a generator with contextlib:**

```python
from contextlib import contextmanager

@contextmanager
def custom_open(filename):
    f = open(filename)
    try:
        yield f
    finally:
        f.close()

with custom_open('file') as f:
    contents = f.read()
```

Use the class approach when there's considerable logic to encapsulate. Use the function approach for simple actions.

## Dynamic Typing

Python is dynamically typed — variables do not have a fixed type. Variables are "tags" or "names" pointing to objects. This flexibility can lead to hard-to-debug code if abused.

Guidelines:
- Avoid using the same variable name for different things.
- Use descriptive names that reflect the type/purpose.

**Bad**

```python
a = 1
a = 'a string'
def a():
    pass
```

**Good**

```python
count = 1
msg = 'a string'
def func():
    pass
```

Using short functions or methods helps reduce the risk of using the same name for unrelated things.

Python supports optional type hints via [PEP 484](https://peps.python.org/pep-0484/), which can be checked with tools like [mypy](https://mypy.readthedocs.io/) or [pyright](https://github.com/microsoft/pyright). Type hints don't change runtime behavior but catch bugs during development:

```python
def greet(name: str) -> str:
    return f"Hello, {name}"
```

## Mutable and Immutable Types

Python has two kinds of built-in types.

**Mutable types** allow in-place modification: lists, dictionaries, sets.

**Immutable types** provide no method for changing their content: integers, strings, tuples.

```python
my_list = [1, 2, 3]
my_list[0] = 4
print(my_list)  # [4, 2, 3] — the same list has changed

x = 6
x = x + 1  # The new x is a different object
```

Mutable types cannot be used as dictionary keys. Use the immutable equivalent — tuples — instead.

Strings are immutable. When constructing a string from parts, use `join()` rather than concatenation:

**Bad**

```python
nums = ""
for n in range(20):
    nums += str(n)  # slow, creates a new string each time
```

**Best**

```python
nums = [str(n) for n in range(20)]
print("".join(nums))
```

For simple concatenation of a few strings, `+` is fine. For building strings from many parts, use `join()`. And for string interpolation, use f-strings:

```python
name = "World"
greeting = f"Hello, {name}!"
```

## Further Reading

- [Python Documentation](https://docs.python.org/3/library/)
- [Dive Into Python 3](https://diveintopython3.net/)
- [Structuring Your Project on realpython.com](https://realpython.com/python-application-layouts/)
