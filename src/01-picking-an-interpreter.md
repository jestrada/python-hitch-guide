# Choosing a Python Interpreter

Python is not just a language — it's a specification with multiple implementations. Choosing the right interpreter depends on your project's needs: compatibility, performance, and ecosystem.

## CPython (Recommended)

[CPython](https://www.python.org/) is the reference implementation of Python, written in C. It compiles Python code to intermediate bytecode which is then interpreted by a virtual machine. CPython provides the highest level of compatibility with Python packages and C extension modules.

**Use CPython unless you have a specific reason not to.** It's the standard, the most widely tested, and what the vast majority of the Python ecosystem targets.

- If you are writing open source Python code and want to reach the widest possible audience, targeting CPython is best.
- To use packages which rely on C extensions to function, CPython is your only practical option.
- All versions of the Python language are defined by CPython's behavior since it is the reference implementation.

**Current recommended version:** Python 3.12 or 3.13. Each new version brings improved standard library modules, performance gains, security fixes, and better error messages. Python 3.13 introduced a free-threaded build (no-GIL) as an experimental feature.

Install the latest version with [uv](https://docs.astral.sh/uv/):

```bash
$ uv python install 3.13
```

Or use [pyenv](https://github.com/pyenv/pyenv) if you need to manage multiple versions:

```bash
$ pyenv install 3.13
```

## PyPy

[PyPy](https://pypy.org/) is a Python interpreter implemented in a restricted statically-typed subset of the Python language called RPython. The interpreter features a just-in-time (JIT) compiler and supports multiple back-ends (C, CLI, JVM).

PyPy aims for maximum compatibility with CPython while significantly improving performance. On a suite of benchmarks, it's consistently [5x or more faster than CPython](https://speed.pypy.org/).

If you have a CPU-bound Python application and need a performance boost without rewriting code, PyPy is worth trying. It targets modern Python 3.x.

**Caveat:** Some C extension packages may not work with PyPy. Test your dependencies first.

## GraalPy

[GraalPy](https://www.graalvm.org/python/) is a Python implementation built on Oracle's GraalVM. It provides high performance through GraalVM's JIT compiler and offers seamless interoperability with Java, JavaScript, Ruby, and other GraalVM-supported languages.

GraalPy is a good choice when:

- You need to embed Python in a JVM application
- You want polyglot interoperability between Python and other languages
- You're running in a GraalVM environment already

It targets Python 3.x compatibility and is actively developed.

## Jython

[Jython](https://www.jython.org/) compiles Python code to Java bytecode which is then executed by the JVM. It can import and use any Java class like a Python module.

Jython is useful if you need to interface with an existing Java codebase or have other reasons to write Python code for the JVM. However, note that Jython only supports Python 2.7 and development has slowed considerably. For new JVM-based Python work, consider GraalPy instead.

## IronPython

[IronPython](https://ironpython.net/) is an implementation of Python for the .NET framework. It can use both Python and .NET framework libraries, and can expose Python code to other .NET languages.

IronPython supports Python 2.7, with IronPython 3 under development. It remains useful for specific .NET integration scenarios.

## Python for .NET (pythonnet)

[Python for .NET](https://pythonnet.github.io/) takes a different approach from IronPython — it provides near-seamless integration of a natively installed CPython with the .NET Common Language Runtime (CLR). This lets you use standard CPython with full package ecosystem access while also calling into .NET libraries.

It is compatible with Python 3.x and can run alongside IronPython without conflict.

## Which Should You Use?

| Implementation | Best For | Python 3 Support |
|---|---|---|
| **CPython** | General purpose — use by default | ✅ Latest |
| **PyPy** | CPU-bound workloads needing speed | ✅ Yes |
| **GraalPy** | JVM/polyglot environments | ✅ Yes |
| **Jython** | Legacy Java integration | ❌ Python 2.7 only |
| **IronPython** | .NET integration | ⚠️ Python 2.7 (v3 in dev) |
| **pythonnet** | .NET integration with CPython packages | ✅ Yes |

When in doubt, use CPython. It's what the vast majority of Python developers use, what CI systems expect, and what package authors test against.
