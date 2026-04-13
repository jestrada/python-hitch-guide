# Your Development Environment

![image](https://docs.python-guide.org/_static/photos/33175624924_7febc46cc4_k_d.jpg)

## Editors & IDEs

Just about anything that can edit plain text will work for writing Python code; however, using a more powerful editor will make your life much easier.

### VS Code (Recommended)

[Visual Studio Code](https://code.visualstudio.com/) is the most popular editor for Python development today. It's free, lightweight, and has excellent Python support via the official [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).

Key features:
- IntelliSense (autocompletion with type checking)
- Integrated debugging and testing
- Jupyter notebook support
- Linting with [Ruff](https://docs.astral.sh/ruff/), Pylance type checking
- Remote development (SSH, containers, WSL)
- Integrated terminal

Install the [Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python) and optionally [Ruff](https://marketplace.visualstudio.com/items?itemName=charliermarsh.ruff) for fast linting and formatting.

### Cursor

[Cursor](https://cursor.sh/) is a VS Code-based editor with built-in AI assistance. It supports all VS Code extensions and adds AI-powered code completion, editing, and chat. A strong choice if you want AI integrated into your workflow.

### PyCharm

[PyCharm](https://www.jetbrains.com/pycharm/) by JetBrains is a full-featured Python IDE. It has two versions: Professional (paid) and Community (free, Apache 2.0). PyCharm provides excellent refactoring, debugging, testing, and database tools out of the box.

Most of PyCharm's features are also available in IntelliJ IDEA via the free [Python plugin](https://plugins.jetbrains.com/plugin/631-python).

### Zed

[Zed](https://zed.dev/) is a fast, modern editor built in Rust with native Python support, AI-assisted editing, and collaborative features. Still maturing but already excellent for Python development.

### Vim / Neovim

Vim and [Neovim](https://neovim.io/) remain powerful choices for developers who prefer keyboard-driven editing. A good starting configuration for Python in your `~/.vimrc` or Neovim config:

```vim
set textwidth=79
set shiftwidth=4
set tabstop=4
set expandtab
set softtabstop=4
set shiftround
set autoindent
```

For a modern Neovim Python setup, consider these plugins:
- [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig) — LSP client (for pyright/pylsp)
- [none-ls](https://github.com/nvimtools/none-ls.nvim) — formatting and linting (for ruff)
- [nvim-cmp](https://github.com/hrsh7th/nvim-cmp) — autocompletion

### Emacs

Emacs is another powerful, programmable editor. See [Python Programming in Emacs](https://www.emacswiki.org/emacs/PythonProgrammingInEmacs) at EmacsWiki for setup guides.

### Sublime Text

[Sublime Text](https://www.sublimetext.com/) is a fast, polished editor with Python support and a vibrant plugin ecosystem. The [LSP](https://github.com/sublimelsp/LSP) plugin enables modern language server integration.

## Linting & Formatting

### Ruff (Recommended)

[Ruff](https://docs.astral.sh/ruff/) is a fast Python linter and formatter written in Rust. It replaces over a dozen tools (pycodestyle, flake8, isort, autopep8, yapf, pydocstyle, and more) with a single binary that's 10-100x faster.

```bash
$ uv tool install ruff
$ ruff check .          # lint
$ ruff format .         # format
```

Configuration goes in `pyproject.toml`:

```toml
[tool.ruff]
line-length = 88
target-version = "py312"

[tool.ruff.lint]
select = ["E", "F", "I", "N", "W", "UP"]
```

### Black

[Black](https://github.com/psf/black) is the uncompromising Python code formatter. If you're not using Ruff, Black is the standard choice:

```bash
$ uv tool install black
$ black .
```

### Type Checkers

Static type checking catches bugs before runtime:

- [pyright](https://github.com/microsoft/pyright) / [basedpyright](https://github.com/DetachHead/basedpyright) — fast, VS Code's default
- [mypy](https://mypy.readthedocs.io/) — the original, thorough but slower

```bash
$ uv add --dev pyright
$ uv run pyright
```

## Interpreter Tools

### Virtual Environments

Virtual environments isolate project package dependencies. See the [Package & Virtual Environment Management](./03-pipenv-virtualenvs.md) chapter for full details.

### pyenv

[pyenv](https://github.com/pyenv/pyenv) lets you install and switch between multiple Python versions. It works by inserting shims into your PATH:

```bash
$ pyenv install 3.13
$ pyenv install 3.12
$ pyenv global 3.13   # set default
```

pyenv installs CPython, PyPy, Anaconda, miniconda, and other interpreters.

For managing virtual environments with pyenv, use [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv).

> **Note:** If you use uv, it can manage Python installations too (`uv python install`), making pyenv optional.

## Interactive Shells

### IPython

[IPython](https://ipython.org/) provides a rich interactive Python shell with syntax highlighting, autocompletion, magic commands, and more:

```bash
$ uv tool install ipython
$ ipython
```

### bpython

[bpython](https://bpython-interpreter.org/) is an alternative interface with in-line syntax highlighting, readline-like autocomplete, and a "rewind" feature:

```bash
$ uv tool install bpython
$ bpython
```

### ptpython

[ptpython](https://github.com/prompt-toolkit/ptpython) is a REPL built on prompt_toolkit with syntax highlighting, multiline editing, and Vi/Emacs modes:

```bash
$ uv tool install ptpython
$ ptpython
```
