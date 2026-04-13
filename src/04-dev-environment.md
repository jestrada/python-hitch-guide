# Your Development Environment

![image](https://docs.python-guide.org/_static/photos/33175624924_7febc46cc4_k_d.jpg)

## Text Editors

Just about anything that can edit plain text will work for writing Python code; however, using a more powerful editor may make your life a bit easier.

### Vim

Vim is a text editor which uses keyboard shortcuts for editing instead of menus or icons. There are a couple of plugins and settings for the Vim editor to aid Python development. If you only develop in Python, a good start is to set the default settings for indentation and line-wrapping to values compliant with `8`{.interpreted-text role="pep"}. In your home directory, open a file called `.vimrc`{.interpreted-text role="file"} and add the following lines:

    set textwidth=79  " lines longer than 79 columns will be broken
    set shiftwidth=4  " operation >> indents 4 columns; << unindents 4 columns
    set tabstop=4     " a hard TAB displays as 4 columns
    set expandtab     " insert spaces when hitting TABs
    set softtabstop=4 " insert/delete 4 spaces when hitting a TAB/BACKSPACE
    set shiftround    " round indent to multiple of 'shiftwidth'
    set autoindent    " align the new line indent with the previous line

With these settings, newlines are inserted after 79 characters and indentation is set to 4 spaces per tab. If you also use Vim for other languages, there is a handy plugin called [indent](http://www.vim.org/scripts/script.php?script_id=974), which handles indentation settings for Python source files.

There is also a handy syntax plugin called [syntax](http://www.vim.org/scripts/script.php?script_id=790) featuring some improvements over the syntax file included in Vim 6.1.

These plugins supply you with a basic environment for developing in Python. To get the most out of Vim, you should continually check your code for syntax errors and PEP8 compliance. Luckily [pycodestyle](https://pypi.org/project/pycodestyle/) and [Pyflakes](http://pypi.org/project/pyflakes/) will do this for you. If your Vim is compiled with `+python` you can also utilize some very handy plugins to do these checks from within the editor.

For PEP8 checking and pyflakes, you can install [vim-flake8](https://github.com/nvie/vim-flake8). Now you can map the function `Flake8` to any hotkey or action you want in Vim. The plugin will display errors at the bottom of the screen, and provide an easy way to jump to the corresponding line. It"s very handy to call this function whenever you save a file. In order to do this, add the following line to your `.vimrc`{.interpreted-text role="file"}:

    autocmd BufWritePost *.py call Flake8()

If you are already using [syntastic](https://github.com/vim-syntastic/syntastic), you can set it to run Pyflakes on write and show errors and warnings in the quickfix window. An example configuration to do that which also shows status and warning messages in the statusbar would be:

    set statusline+=%#warningmsg#
    set statusline+=%{SyntasticStatuslineFlag()}
    set statusline+=%*
    let g:syntastic_auto_loc_list=1
    let g:syntastic_loc_list_height=5

#### Python-mode

[Python-mode](https://github.com/python-mode/python-mode) is a complex solution for working with Python code in Vim. It has:

- Asynchronous Python code checking (`pylint`, `pyflakes`, `pycodestyle`, `mccabe`) in any combination
- Code refactoring and autocompletion with Rope
- Fast Python folding
- Virtualenv support
- Search through Python documentation and run Python code
- Auto [pycodestyle](https://pypi.org/project/pycodestyle/) error fixes

And more.

#### SuperTab

[SuperTab](http://www.vim.org/scripts/script.php?script_id=1643) is a small Vim plugin that makes code completion more convenient by using `<Tab>` key or any other customized keys.

### Emacs

Emacs is another powerful text editor. It is fully programmable (Lisp), but it can be some work to wire up correctly. A good start if you"re already an Emacs user is [Python Programming in Emacs](https://www.emacswiki.org/emacs/PythonProgrammingInEmacs) at EmacsWiki.

1.  Emacs itself comes with a Python mode.

### TextMate

> [TextMate](http://macromates.com/) brings Apple"s approach to operating systems into the world of text editors. By bridging Unix underpinnings and GUI, TextMate cherry-picks the best of both worlds to the benefit of expert scripters and novice users alike.

### Sublime Text

> [Sublime Text](http://www.sublimetext.com/) is a sophisticated text editor for code, markup, and prose. You"ll love the slick user interface, extraordinary features, and amazing performance.

Sublime Text has excellent support for editing Python code and uses Python for its plugin API. It also has a diverse variety of plugins, [some of which](https://github.com/SublimeLinter/SublimeLinter) allow for in-editor PEP8 checking and code "linting".

### Atom

> [Atom](https://atom.io/) is a hackable text editor for the 21st century, built on atom-shell, and based on everything we love about our favorite editors.

Atom is web native (HTML, CSS, JS), focusing on modular design and easy plugin development. It comes with native package control and a plethora of packages. Recommended for Python development is [Linter](https://github.com/steelbrain/linter) combined with [linter-flake8](https://github.com/AtomLinter/linter-flake8).

### Python (on Visual Studio Code)

[Python for Visual Studio](https://marketplace.visualstudio.com/items?itemName=ms-python.python) is an extension for the [Visual Studio Code](https://code.visualstudio.com). This is a free, lightweight, open source code editor, with support for Mac, Windows, and Linux. Built using open source technologies such as Node.js and Python, with compelling features such as Intellisense (autocompletion), local and remote debugging, linting, and the like.

MIT licensed.

## IDEs

### PyCharm / IntelliJ IDEA

[PyCharm](http://www.jetbrains.com/pycharm/) is developed by JetBrains, also known for IntelliJ IDEA. Both share the same code base and most of PyCharm"s features can be brought to IntelliJ with the free [Python Plug-In](https://plugins.jetbrains.com/plugin/?idea&pluginId=631). There are two versions of PyCharm: Professional Edition (Free 30-day trial) and Community Edition (Apache 2.0 License) with fewer features.

### Enthought Canopy

[Enthought Canopy](https://www.enthought.com/product/canopy/) is a Python IDE which is focused towards Scientists and Engineers as it provides pre installed libraries for data analysis.

### Eclipse

The most popular Eclipse plugin for Python development is Aptana"s [PyDev](https://pydev.org).

### Komodo IDE

[Komodo IDE](https://www.activestate.com/products/komodo-ide/) is developed by ActiveState and is a commercial IDE for Windows, Mac, and Linux. [KomodoEdit](https://github.com/Komodo/KomodoEdit) is the open source alternative.

### Spyder

[Spyder](https://github.com/spyder-ide/spyder) is an IDE specifically geared toward working with scientific Python libraries (namely [SciPy](https://www.scipy.org/)). It includes integration with [pyflakes](http://pypi.org/project/pyflakes/), [pylint](https://www.logilab.org/857) and [rope](https://github.com/python-rope/rope).

Spyder is open source (free), offers code completion, syntax highlighting, a class and function browser, and object inspection.

### WingIDE

[WingIDE](http://wingware.com/) is a Python specific IDE. It runs on Linux, Windows, and Mac (as an X11 application, which frustrates some Mac users).

WingIDE offers code completion, syntax highlighting, source browser, graphical debugger and support for version control systems.

### NINJA-IDE

[NINJA-IDE](http://www.ninja-ide.org/) (from the recursive acronym: "Ninja-IDE Is Not Just Another IDE") is a cross-platform IDE, specially designed to build Python applications, and runs on Linux/X11, Mac OS X, and Windows desktop operating systems. Installers for these platforms can be downloaded from the website.

NINJA-IDE is open source software (GPLv3 licence) and is developed in Python and Qt. The source files can be downloaded from [GitHub](https://github.com/ninja-ide).

### Eric (The Eric Python IDE)

[Eric](http://eric-ide.python-projects.org/) is a full featured Python IDE offering source code autocompletion, syntax highlighting, support for version control systems, Python 3 support, integrated web browser, python shell, integrated debugger, and a flexible plug-in system. Written in Python, it is based on the Qt GUI toolkit, integrating the Scintilla editor control. Eric is an open source software project (GPLv3 licence) with more than ten years of active development.

### Mu

[Mu](https://codewith.mu/) is a minimalist Python IDE which can run Python 3 code locally and can also deploy code to the BBC micro:bit and to Adafruit boards running CircuitPython.

Intended for beginners, mu includes a Python 3 interpreter, and is easy to install on Windows, OS/X and Linux. It runs well on the Raspberry Pi.

There"s an active support community on gitter.

## Interpreter Tools

### Virtual Environments

Virtual Environments provide a powerful way to isolate project package dependencies. This means that you can use packages particular to a Python project without installing them system wide and thus avoiding potential version conflicts.

To start using and see more information: [Virtual Environments](https://github.com/kennethreitz/python-guide/blob/master/docs/dev/virtualenvs.rst) docs.

### pyenv

[pyenv](https://github.com/pyenv/pyenv) is a tool to allow multiple versions of the Python interpreter to be installed at the same time. This solves the problem of having different projects requiring different versions of Python. For example, it becomes very easy to install Python 2.7 for compatibility in one project, while still using Python 3.4 as the default interpreter. pyenv isn"t just limited to the CPython versions -- it will also install PyPy, Anaconda, miniconda, stackless, Jython, and IronPython interpreters.

pyenv works by filling a `shims` directory with fake versions of the Python interpreter (plus other tools like `pip` and `2to3`). When the system looks for a program named `python`, it looks inside the `shims` directory first, and uses the fake version, which in turn passes the command on to pyenv. pyenv then works out which version of Python should be run based on environment variables, `.python-version` files, and the global default.

pyenv isn"t a tool for managing virtual environments, but there is the plugin [pyenv-virtualenv](https://github.com/pyenv/pyenv-virtualenv) which automates the creation of different environments, and also makes it possible to use the existing pyenv tools to switch to different environments based on environment variables or `.python-version` files.

## Other Tools

### IDLE

`IDLE <python:idle>`{.interpreted-text role="ref"} is an integrated development environment that is part of the Python standard distribution. It is completely written in Python and uses the Tkinter GUI toolkit. Though IDLE is not suited for full-blown development using Python, it is quite helpful to try out small Python snippets and experiment with different features in Python.

It provides the following features:

- Python Shell Window (interpreter)
- Multi window text editor that colorizes Python code
- Minimal debugging facility

### IPython

[IPython](http://ipython.org/) provides a rich toolkit to help you make the most out of using Python interactively. Its main components are:

- Powerful Python shells (terminal- and Qt-based)
- A web-based notebook with the same core features but support for rich media, text, code, mathematical expressions and inline plots
- Support for interactive data visualization and use of GUI toolkits
- Flexible, embeddable interpreters to load into your own projects
- Tools for high level and interactive parallel computing

``` console
$ pip install ipython
```

To download and install IPython with all its optional dependencies for the notebook, qtconsole, tests, and other functionalities:

``` console
$ pip install ipython[all]
```

### BPython

[bpython](https://bpython-interpreter.org/) is an alternative interface to the Python interpreter for Unix-like operating systems. It has the following features:

- In-line syntax highlighting
- Readline-like autocomplete with suggestions displayed as you type
- Expected parameter list for any Python function
- "Rewind" function to pop the last line of code from memory and re-evaluate
- Send entered code off to a pastebin
- Save entered code to a file
- Auto-indentation
- Python 3 support

``` console
$ pip install bpython
```

### ptpython

[ptpython](https://github.com/prompt-toolkit/ptpython) is a REPL build on top of the [prompt_toolkit](https://github.com/prompt-toolkit/python-prompt-toolkit) library. It is considered to be an alternative to [BPython](https://bpython-interpreter.org/). Features include:

- Syntax highlighting
- Autocompletion
- Multiline editing
- Emacs and Vim Modes
- Embedding REPL inside of your code
- Syntax validation
- Tab pages
- Support for integrating with [IPython](http://ipython.org/)"s shell, by installing IPython (`pip install ipython`) and running `ptipython`.

``` console
$ pip install ptpython
```
