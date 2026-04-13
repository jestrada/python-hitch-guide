# Installing Python

![image](https://docs.python-guide.org/_static/photos/36137232412_fdcb0f84eb_k_d.jpg)

There's a good chance you already have Python on your operating system. If so, you may not need to install anything else to use Python. That said, I strongly recommend installing the tools described in the guides below before you start building Python applications for real-world use.

## The Easiest Way: uv

[uv](https://docs.astral.sh/uv/) is a fast, all-in-one Python tool that manages Python installations, virtual environments, and packages. It's the recommended way to get started.

Install uv:

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

Then install the latest Python:

```bash
$ uv python install 3.13
```

uv can install and manage multiple Python versions, create virtual environments, install packages, and run scripts — all without needing separate tools.

## Installation by Platform

### Python 3 on macOS

Install via [Homebrew](https://brew.sh):

```bash
$ brew install python@3.13
```

Or use uv:

```bash
$ uv python install 3.13
```

Or download the installer from [python.org](https://www.python.org/downloads/).

### Python 3 on Windows

Install via the [Microsoft Store](https://apps.microsoft.com/detail/9NCVDN91XZQP) (search "Python"), or download the installer from [python.org](https://www.python.org/downloads/). During installation, check **"Add Python to PATH"**.

Or use uv:

```powershell
$ uv python install 3.13
```

### Python 3 on Linux

Most Linux distributions include Python 3. Install it with your package manager if it's not already present:

**Debian / Ubuntu:**
```bash
$ sudo apt update
$ sudo apt install python3 python3-pip python3-venv
```

**Fedora:**
```bash
$ sudo dnf install python3 python3-pip
```

**Arch:**
```bash
$ sudo pacman -S python python-pip
```

Or use uv for a version-independent install:

```bash
$ uv python install 3.13
```

## Managing Multiple Versions

If you need multiple Python versions side by side:

**With uv:**
```bash
$ uv python install 3.12
$ uv python install 3.13
$ uv python list  # see installed versions
```

**With pyenv (macOS/Linux):**
```bash
$ brew install pyenv  # or see pyenv docs for Linux
$ pyenv install 3.13
$ pyenv install 3.12
$ pyenv global 3.13  # set default
```

## Verifying Your Installation

```bash
$ python3 --version
Python 3.13.x

$ pip3 --version
pip 24.x from ... (python 3.13)
```

You're ready to go. Next up: [setting up virtual environments](./03-pipenv-virtualenvs.md).
