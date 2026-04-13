# Package Manager Configuration

![image](https://docs.python-guide.org/_static/photos/34018732105_f0e6758859_k_d.jpg)

## uv Configuration

[uv](https://docs.astral.sh/uv/) is the recommended package manager. Most configuration lives in `pyproject.toml` and `uv.lock`, but some behaviors can be configured via environment variables.

### Global Cache

uv caches downloaded packages by default in `~/.cache/uv/` (macOS/Linux) or `%LOCALAPPDATA%\uv\cache` (Windows). This means packages are downloaded once and shared across all projects and virtual environments.

To clear the cache:

```bash
$ uv cache clean
```

To set a custom cache directory:

```bash
$ export UV_CACHE_DIR=/path/to/cache
```

### Requiring an Active Environment

When using pip directly (not uv), it's easy to accidentally install packages globally. To prevent this, add to your `~/.bashrc` or `~/.zshrc`:

```bash
export PIP_REQUIRE_VIRTUALENV=true
```

Then pip will refuse to install outside a virtual environment:

```bash
$ pip install requests
Could not find an activated virtualenv (required).
```

Override it when you need a global install:

```bash
gpip() {
    PIP_REQUIRE_VIRTUALENV=false pip "$@"
}
```

### pip Configuration

If you use pip alongside uv, you can configure it via `pip.conf` (macOS/Linux) or `pip.ini` (Windows):

**macOS / Linux:** `~/.pip/pip.conf`
**Windows:** `%USERPROFILE%\pip\pip.ini`

Example configuration:

```ini
[global]
require-virtualenv = true
```

### Caching with pip

Modern pip (6.0+) has built-in caching enabled by default — no configuration needed. If you're on an older version, upgrade:

```bash
$ pip install --upgrade pip
```

## Summary: uv vs pip

| Task | uv | pip |
|------|-----|-----|
| Install package | `uv add requests` | `pip install requests` |
| Install (one-time) | `uv pip install requests` | `pip install requests` |
| Lock dependencies | Automatic (`uv.lock`) | Manual (`pip freeze > requirements.txt`) |
| Create venv | Automatic or `uv venv` | `python3 -m venv .venv` |
| Run in env | `uv run <command>` | Activate venv first |
| Global tools | `uv tool install <pkg>` | `pipx install <pkg>` |
| Cache | Built-in, fast | Built-in |

Use uv for projects. Use `uv pip` when you need pip-compatible commands. Use `uv tool` for global CLI tools.
