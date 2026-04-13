# Code Style

![image](https://docs.python-guide.org/_static/photos/33907150054_5ee79e8940_k_d.jpg)

If you ask Python programmers what they like most about Python, they will often cite its high readability. Indeed, a high level of readability is at the heart of the design of the Python language, following the recognized fact that code is read much more often than it is written.

One reason for the high readability of Python code is its relatively complete set of Code Style guidelines and "Pythonic" idioms.

When a veteran Python developer (a Pythonista) calls portions of code not "Pythonic", they usually mean that these lines of code do not follow the common guidelines and fail to express its intent in what is considered the best (hear: most readable) way.

## General Concepts

### Explicit Code

While any kind of black magic is possible with Python, the most explicit and straightforward manner is preferred.

**Bad**

```python
def make_complex(*args):
    x, y = args
    return dict(**locals())
```

**Good**

```python
def make_complex(x, y):
    return {'x': x, 'y': y}
```

In the good code above, x and y are explicitly received from the caller, and an explicit dictionary is returned. The developer using this function knows exactly what to do by reading the first and last lines.

### One Statement Per Line

While compound statements such as list comprehensions are appreciated for their brevity, it is bad practice to have two disjointed statements on the same line.

**Bad**

```python
print('one'); print('two')

if x == 1: print('one')

if <complex comparison> and <other complex comparison>:
    # do something
```

**Good**

```python
print('one')
print('two')

if x == 1:
    print('one')

cond1 = <complex comparison>
cond2 = <other complex comparison>
if cond1 and cond2:
    # do something
```

### Function Arguments

Arguments can be passed to functions in four different ways:

1. **Positional arguments** are mandatory and have no default values. For instance, in `send(message, recipient)` or `point(x, y)`.

2. **Keyword arguments** are not mandatory and have default values. When a function has more than two or three positional parameters, keyword arguments with default values are helpful. For instance: `send(message, to, cc=None, bcc=None)`.

   As a side note, following the [YAGNI](https://en.wikipedia.org/wiki/You_aren%27t_gonna_need_it) principle, it's often harder to remove an optional argument that was added "just in case" than to add one when needed.

3. **Arbitrary argument list** (`*args`): If the function's intention is better expressed with an extensible number of positional arguments. However, if a function receives a list of arguments of the same nature, it's often clearer to define it as a function of one list argument.

4. **Arbitrary keyword argument dictionary** (`**kwargs`): For an undetermined series of named arguments. Use these powerful techniques only when there's a proven necessity.

### Avoid the Magical Wand

Python comes with a very rich set of hooks and tools for tricky tricks — changing how objects are created, how the interpreter imports modules, even embedding C routines. However, these options have drawbacks and readability suffers greatly.

Knowing how and particularly when **not** to use them is very important. Like a kung fu master, a Pythonista knows how to kill with a single finger, and never to actually do it.

### We Are All Responsible Users

Python allows many tricks, and some are potentially dangerous. Any client code can override an object's properties and methods — there is no "private" keyword. This philosophy is expressed by: "We are all responsible users."

The main convention for private properties and implementation details is to prefix all "internals" with an underscore. If client code breaks this rule, any misbehavior is the client code's responsibility.

Using this convention generously is encouraged: any method or property not intended for client code should be prefixed with an underscore.

### Returning Values

When a function grows in complexity, it's common to use multiple return statements. However, to keep a clear intent:

- Return early for error cases (returning `None` or raising an exception)
- Keep a single exit point for the main result

```python
def complex_function(a, b, c):
    if not a:
        return None  # Raising an exception might be better
    if not b:
        return None  # Raising an exception might be better
    # Some complex code trying to compute x from a, b and c
    if not x:
        # Some Plan-B computation of x
    return x  # One single exit point for the returned value
```

## Idioms

A programming idiom is a *way* to write code. Idiomatic Python code is often referred to as being *Pythonic*. Some common Python idioms follow:

### Unpacking

If you know the length of a list or tuple, you can assign names to its elements:

```python
for index, item in enumerate(some_list):
    # do something with index and item
```

Swap variables:

```python
a, b = b, a
```

Nested unpacking:

```python
a, (b, c) = 1, (2, 3)
```

Extended unpacking:

```python
a, *rest = [1, 2, 3]
# a = 1, rest = [2, 3]

a, *middle, c = [1, 2, 3, 4]
# a = 1, middle = [2, 3], c = 4
```

### Create an Ignored Variable

If you need to assign something but won't use it, use `__`:

```python
filename = 'foobar.txt'
basename, __, ext = filename.rpartition('.')
```

Many style guides recommend `_` for throwaway variables, but `_` conflicts with `gettext.gettext` and the interactive prompt's last result. Double underscore avoids these conflicts.

### Create a Length-N List of the Same Thing

```python
four_nones = [None] * 4
```

### Create a Length-N List of Lists

Because lists are mutable, `*` creates N references to the *same* list. Use a list comprehension:

```python
four_lists = [[] for __ in range(4)]
```

### Create a String from a List

```python
letters = ['s', 'p', 'a', 'm']
word = ''.join(letters)
```

### Searching for an Item in a Collection

Sets and dictionaries use hashtables for O(1) lookups. Lists require O(n) scanning:

```python
s = {'s', 'p', 'a', 'm'}
l = ['s', 'p', 'a', 'm']

def lookup_set(s):
    return 's' in s  # Fast — O(1)

def lookup_list(l):
    return 's' in l  # Slow for large lists — O(n)
```

Use sets or dictionaries instead of lists when:
- The collection will contain many items
- You will repeatedly search for items
- You don't have duplicate items

### Use f-strings for String Formatting

F-strings (introduced in Python 3.6 by [PEP 498](https://peps.python.org/pep-0498/)) are the preferred way to format strings:

```python
name = "World"
age = 30

# Best — f-strings (Python 3.6+)
greeting = f"Hello, {name}! You are {age} years old."

# OK — .format()
greeting = "Hello, {}! You are {} years old.".format(name, age)

# Avoid — % formatting (old style)
greeting = "Hello, %s! You are %d years old." % (name, age)
```

F-strings support expressions:

```python
f"2 + 2 = {2 + 2}"           # "2 + 2 = 4"
f"{'hello'.upper()}"          # "HELLO"
f"{name!r}"                   # "'World'" (repr)
f"{age:>10}"                  # "        30" (right-aligned)
```

> **Note:** For logging, prefer `%s` style or lazy formatting for performance — f-strings evaluate immediately even if the log level won't emit the message.

## Type Hints

Python supports optional type hints ([PEP 484](https://peps.python.org/pep-0484/)) which improve code clarity and enable static analysis:

```python
def greet(name: str, age: int = 0) -> str:
    return f"Hello, {name}! You are {age} years old."

from typing import Optional

def find_user(user_id: int) -> Optional[dict]:
    """Returns user dict or None if not found."""
    ...
```

For modern Python (3.10+), use the cleaner syntax:

```python
def find_user(user_id: int) -> dict | None:
    ...

names: list[str] = []
config: dict[str, int] = {}
```

Check types with [pyright](https://github.com/microsoft/pyright) or [mypy](https://mypy.readthedocs.io/):

```bash
$ uv add --dev pyright
$ uv run pyright
```

## Zen of Python

Also known as [PEP 20](https://peps.python.org/pep-0020/), the guiding principles for Python's design:

```pycon
>>> import this
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

## PEP 8

[PEP 8](https://peps.python.org/pep-0008/) is the de facto code style guide for Python. A high-quality, easy-to-read version is also available at [pep8.org](https://pep8.org/).

The entire Python community does their best to adhere to these guidelines. Conforming your Python code to PEP 8 is generally a good idea and helps make code more consistent when working with other developers.

## Linting & Formatting Tools

### Ruff (Recommended)

[Ruff](https://docs.astral.sh/ruff/) is a fast Python linter and formatter written in Rust. It replaces pycodestyle, flake8, isort, autopep8, yapf, and dozens of other tools with a single binary:

```bash
$ uv tool install ruff
$ ruff check .          # lint for issues
$ ruff format .         # auto-format
```

Configuration in `pyproject.toml`:

```toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP"]
```

Ruff can also auto-fix issues:

```bash
$ ruff check --fix .
```

### Black

[Black](https://github.com/psf/black) is the uncompromising Python code formatter. It takes the formatting debate out of your hands:

```bash
$ uv tool install black
$ black .
```

Black's formatting is deterministic — everyone on your team gets the same result.

## Conventions

### Check if a Variable Equals a Constant

You don't need to explicitly compare a value to `True`, `None`, or `0`:

**Bad**

```python
if attr == True:
    print('True!')

if attr == None:
    print('attr is None!')
```

**Good**

```python
# Just check the value
if attr:
    print('attr is truthy!')

# Check for the opposite
if not attr:
    print('attr is falsey!')

# Explicitly check for None
if attr is None:
    print('attr is None!')
```

### Access a Dictionary Element

Use `in` or `dict.get()`:

**Bad**

```python
d = {'hello': 'world'}
if d.has_key('hello'):  # has_key was removed in Python 3
    print(d['hello'])
```

**Good**

```python
d = {'hello': 'world'}

print(d.get('hello', 'default_value'))  # prints 'world'
print(d.get('thingy', 'default_value'))  # prints 'default_value'

if 'hello' in d:
    print(d['hello'])
```

### Short Ways to Manipulate Lists

[List comprehensions](https://docs.python.org/3/tutorial/datastructures.html#list-comprehensions) provide a powerful, concise way to work with lists. [Generator expressions](https://docs.python.org/3/tutorial/classes.html#generator-expressions) follow similar syntax but return a generator instead of creating a list.

**Bad**

```python
# Allocates a full list in memory
valedictorian = max([(student.gpa, student.name) for student in graduates])
```

**Good**

```python
valedictorian = max((student.gpa, student.name) for student in graduates)
```

Never use a list comprehension just for side effects:

**Bad**

```python
[print(x) for x in sequence]
```

**Good**

```python
for x in sequence:
    print(x)
```

### Filtering a List

**Bad** — never remove items from a list while iterating:

```python
a = [3, 4, 5]
for i in a:
    if i > 4:
        a.remove(i)
```

**Good** — use a list comprehension or generator expression:

```python
filtered_values = [value for value in sequence if value != x]
# or as a generator:
filtered_values = (value for value in sequence if value != x)
```

### Modifying Values in a List

**Good** — create a new list to avoid side effects:

```python
a = [3, 4, 5]
b = a
a = [i + 3 for i in a]  # b is unchanged
```

Use `enumerate` to track position:

```python
a = [3, 4, 5]
for i, item in enumerate(a):
    print(i, item)
```

### Read From a File

Use the `with open` syntax:

**Bad**

```python
f = open('file.txt')
a = f.read()
print(a)
f.close()
```

**Good**

```python
with open('file.txt') as f:
    for line in f:
        print(line)
```

### Line Continuations

Use parentheses for line continuations instead of backslashes:

**Bad**

```python
my_very_big_string = """For a long time I used to go to bed early. Sometimes, \
    when I had put out my candle, my eyes would close so quickly that I had not even \
    time to say "I'm going to sleep.""""

from some.deep.module.inside.a.module import a_nice_function, another_nice_function, \
    yet_another_nice_function
```

**Good**

```python
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes, "
    "when I had put out my candle, my eyes would close so quickly "
    "that I had not even time to say \"I'm going to sleep.\""
)

from some.deep.module.inside.a.module import (
    a_nice_function, another_nice_function, yet_another_nice_function
)
```

Having to split a long logical line is often a sign you're trying to do too many things at once.
