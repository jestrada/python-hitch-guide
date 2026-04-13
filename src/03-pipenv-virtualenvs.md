# Package & Virtual Environment Management

![image](https://docs.python-guide.org/_static/photos/35294660055_42c02b2316_k_d.jpg)

This tutorial walks you through installing and using Python packages.

It will show you how to install and use the necessary tools and make strong recommendations on best practices. Keep in mind that Python is used for a great many different purposes, and precisely how you want to manage your dependencies may change based on how you decide to publish your software. The guidance presented here is most directly applicable to the development and deployment of network services (including web applications), but is also very well suited to managing development and testing environments for any kind of project.

## Make sure you've got Python

Before you go any further, make sure you have Python and that it's available from your command line:

```bash
$ python3 --version
```

You should get output like `Python 3.13.x`. If you do not have Python, see the [Installing Python](./02-installing-python.md) section of this guide.

> If you're a newcomer and you get an error like `NameError: name 'python' is not defined`, it's because this command is intended to be run in a *shell* (also called a *terminal* or *console*), not inside the Python interpreter.

## uv (Recommended)

[uv](https://docs.astral.sh/uv/) is a fast, all-in-one Python package manager and project tool written in Rust. It replaces pip, virtualenv, pip-tools, pipx, poetry, pyenv, and more — with a single binary that's 10-100x faster than the tools it replaces.

If you're familiar with Node.js's [npm](https://www.npmjs.com/) or Rust's [cargo](https://doc.rust-lang.org/cargo/), it's similar in spirit to those tools.

### Installing uv

**macOS / Linux:**
```bash
$ curl -LsSf https://astral.sh/uv/install.sh | sh
```

**Windows:**
```powershell
$ powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

**With pip:**
```bash
$ pip install uv
```

### Creating a Project

```bash
$ mkdir myproject && cd myproject
$ uv init
```

This creates a `pyproject.toml` and a `.python-version` file. Now add a dependency:

```bash
$ uv add requests
```

uv will:
1. Create a virtual environment automatically (`.venv/`)
2. Resolve and lock all dependencies (`uv.lock`)
3. Install everything

### Using Installed Packages

Create a `main.py` file:

```python
import requests

response = requests.get('https://httpbin.org/ip')
print(f'Your IP is {response.json()["origin"]}')
```

Run it with uv:

```bash
$ uv run python main.py
```

Using `uv run` ensures your script runs within the project's virtual environment with all dependencies available.

### Adding and Removing Dependencies

```bash
$ uv add httpx          # add a dependency
$ uv add --dev pytest   # add a development dependency
$ uv remove httpx       # remove a dependency
```

All changes are reflected in `pyproject.toml` and `uv.lock`.

### Running Commands in the Environment

```bash
$ uv run pytest         # run any command in the virtual environment
$ uv run python app.py  # run a script with dependencies available
$ uv shell              # activate the virtual environment in your shell
```

## pipx: Install Global CLI Tools

[pipx](https://pipx.pypa.io/) installs and runs Python CLI applications in isolated environments. Use it for tools you want available system-wide:

```bash
$ uv tool install black      # install a tool globally
$ uv tool install ruff
$ uv tool run ruff check .   # run without installing
```

## Lower Level: venv

Python 3 includes the built-in [venv](https://docs.python.org/3/library/venv.html) module for creating virtual environments. This is useful when you don't need a full project setup.

### Basic Usage

1. Create a virtual environment:

```bash
$ python3 -m venv .venv
```

2. Activate it:

**macOS / Linux:**
```bash
$ source .venv/bin/activate
```

**Windows:**
```
> .venv\Scripts\activate
```

The name of the active environment appears in your prompt (e.g., `(.venv)`).

3. Install packages:

```bash
$ pip install requests
```

4. Save dependencies:

```bash
$ pip freeze > requirements.txt
```

5. Install from a requirements file:

```bash
$ pip install -r requirements.txt
```

6. Deactivate:

```bash
$ deactivate
```

To delete a virtual environment, simply remove its folder:

```bash
$ rm -rf .venv
```

> Remember to exclude the virtual environment folder from source control by adding `.venv/` to your `.gitignore`.

## direnv

[direnv](https://direnv.net) automatically activates a virtual environment when you `cd` into a project directory. Add this to your project's `.envrc`:

```bash
source .venv/bin/activate
```

Install on macOS:

```bash
$ brew install direnv
```

Then follow the [setup instructions](https://direnv.net/docs/hook.html) for your shell.
