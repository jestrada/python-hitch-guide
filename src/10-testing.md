# Testing Your Code

![image](https://docs.python-guide.org/_static/photos/34435687940_8f73fc1fa6_k_d.jpg)

Testing your code is very important.

Getting used to writing testing code and running it in parallel is now considered a good habit. Used wisely, this method helps define your code's intent more precisely and leads to a more decoupled architecture.

Some general rules of testing:

- A testing unit should focus on one tiny bit of functionality and prove it correct.
- Each test unit must be fully independent. Each test must be able to run alone and within the test suite, regardless of order. Use `setUp()` and `tearDown()` (or setup/teardown fixtures) to manage test data.
- Try hard to make tests that run fast. If a single test needs more than a few milliseconds, development will slow down. Keep heavier tests in a separate suite.
- Learn your tools and learn how to run a single test or test case.
- Always run the full test suite before and after a coding session.
- Implement a hook that runs all tests before pushing code.
- If you're in the middle of development and have to interrupt, write a broken unit test about what you want to develop next. You'll have a pointer when you return.
- The first step when debugging is to write a new test pinpointing the bug.
- Use long and descriptive names for testing functions: `test_square_of_number_2()`, `test_square_negative_number()`.
- Testing code is read as much as running code. A unit test with unclear purpose is not very helpful.
- Testing code serves as an introduction to new developers. Running and reading tests is often the best way to learn a codebase.

## The Basics

### unittest

[unittest](https://docs.python.org/3/library/unittest.html) is the batteries-included test module in the Python standard library.

```python
import unittest

def fun(x):
    return x + 1

class MyTest(unittest.TestCase):
    def test(self):
        self.assertEqual(fun(3), 4)
```

### Doctest

The [doctest](https://docs.python.org/3/library/doctest.html) module searches for interactive Python sessions in docstrings and verifies they work as shown.

Doctests are useful as expressive documentation of main use cases, though they're less detailed than proper unit tests:

```python
def square(x):
    """Return the square of x.

    >>> square(2)
    4
    >>> square(-2)
    4
    """
    return x * x

if __name__ == '__main__':
    import doctest
    doctest.testmod()
```

## Tools

### pytest (Recommended)

[pytest](https://docs.pytest.org/) is a no-boilerplate alternative to unittest. It's the most popular Python testing framework:

```bash
$ uv add --dev pytest
```

Creating a test suite is as easy as writing a module with functions:

```python
# content of test_sample.py
def func(x):
    return x + 1

def test_answer():
    assert func(3) == 5
```

Run with:

```bash
$ uv run pytest
=========================== test session starts ============================
platform darwin -- Python 3.13.0 -- pytest-8.3.4
collected 1 items

test_sample.py F

================================= FAILURES =================================
_______________________________ test_answer ________________________________

    def test_answer():
>       assert func(3) == 5
E       assert 4 == 5
E        +  where 4 = func(3)

test_sample.py:5: AssertionError
========================= 1 failed in 0.02 seconds =========================
```

Far less work than the equivalent with unittest!

### Hypothesis

[Hypothesis](https://hypothesis.readthedocs.io/) lets you write tests parameterized by a source of examples. It generates simple, comprehensible examples that make your tests fail:

```bash
$ uv add --dev hypothesis
```

```python
from hypothesis import given, strategies as st

@given(st.lists(st.floats(allow_nan=False, allow_infinity=False), min_size=1))
def test_mean(xs):
    mean = sum(xs) / len(xs)
    assert min(xs) <= mean <= max(xs)
```

Hypothesis will find bugs that escape all other forms of testing.

### tox

[tox](https://tox.readthedocs.io/) automates test environment management and testing against multiple Python versions:

```bash
$ uv add --dev tox
```

Configure in `pyproject.toml` or `tox.ini`. Modern alternative: [nox](https://nox.thea.codes/).

### unittest.mock

[unittest.mock](https://docs.python.org/3/library/unittest.mock.html) is in the standard library since Python 3.3 — no installation needed.

It allows you to replace parts of your system under test with mock objects:

```python
from unittest.mock import MagicMock

thing = ProductionClass()
thing.method = MagicMock(return_value=3)
thing.method(3, 4, 5, key='value')

thing.method.assert_called_with(3, 4, 5, key='value')
```

Use the `patch` decorator to mock classes or objects during a test:

```python
from unittest.mock import patch

def mock_search(self):
    class MockSearchQuerySet(SearchQuerySet):
        def __iter__(self):
            return iter(["foo", "bar", "baz"])
    return MockSearchQuerySet()

@patch('myapp.SearchForm.search', mock_search)
def test_new_watchlist_activities(self):
    self.assertEqual(len(myapp.get_search_results(q="fish")), 3)
```
