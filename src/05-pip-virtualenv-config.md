# Further Configuration of pip and Virtualenv {#pip-virtualenv}

![image](https://docs.python-guide.org/_static/photos/34018732105_f0e6758859_k_d.jpg)

## Requiring an active virtual environment for `pip`

By now it should be clear that using virtual environments is a great way to keep your development environment clean and keeping different projects" requirements separate.

When you start working on many different projects, it can be hard to remember to activate the related virtual environment when you come back to a specific project. As a result of this, it is very easy to install packages globally while thinking that you are actually installing the package for the virtual environment of the project. Over time this can result in a messy global package list.

In order to make sure that you install packages to your active virtual environment when you use `pip install`, consider adding the following line to your `~/.bashrc`{.interpreted-text role="file"} file:

``` console
export PIP_REQUIRE_VIRTUALENV=true
```

After saving this change and sourcing the `~/.bashrc`{.interpreted-text role="file"} file with `source ~/.bashrc`, pip will no longer let you install packages if you are not in a virtual environment. If you try to use `pip install` outside of a virtual environment pip will gently remind you that an activated virtual environment is needed to install packages.

``` console
$ pip install requests
Could not find an activated virtualenv (required).
```

You can also do this configuration by editing your `pip.conf`{.interpreted-text role="file"} or `pip.ini`{.interpreted-text role="file"} file. `pip.conf`{.interpreted-text role="file"} is used by Unix and Mac OS X operating systems and it can be found at:

``` console
$HOME/.pip/pip.conf
```

Similarly, the `pip.ini`{.interpreted-text role="file"} file is used by Windows operating systems and it can be found at:

``` console
%USERPROFILE%\pip\pip.ini
```

If you don"t have a `pip.conf`{.interpreted-text role="file"} or `pip.ini`{.interpreted-text role="file"} file at these locations, you can create a new file with the correct name for your operating system.

If you already have a configuration file, just add the following line under the `[global]` settings to require an active virtual environment:

``` console
require-virtualenv = true
```

If you did not have a configuration file, you will need to create a new one and add the following lines to this new file:

``` console
[global]
require-virtualenv = true
```

You will of course need to install some packages globally (usually ones that you use across different projects consistently) and this can be accomplished by adding the following to your `~/.bashrc`{.interpreted-text role="file"} file:

``` console
gpip() {
    PIP_REQUIRE_VIRTUALENV=false pip "$@"
}
```

After saving the changes and sourcing your `~/.bashrc`{.interpreted-text role="file"} file you can now install packages globally by running `gpip install`. You can change the name of the function to anything you like, just keep in mind that you will have to use that name when trying to install packages globally with pip.

## Caching packages for future use

Every developer has preferred libraries and when you are working on a lot of different projects, you are bound to have some overlap between the libraries that you use. For example, you may be using the `requests` library in a lot of different projects.

It is surely unnecessary to re-download the same packages/libraries each time you start working on a new project (and in a new virtual environment as a result). Fortunately, starting with version 6.0, pip provides an [on-by-default caching mechanism](https://pip.pypa.io/en/stable/reference/pip_install/#caching) that doesn"t need any configuration.

When using older versions, you can configure pip in such a way that it tries to reuse already installed packages, too.

On Unix systems, you can add the following line to your `.bashrc`{.interpreted-text role="file"} or `.bash_profile`{.interpreted-text role="file"} file.

``` console
export PIP_DOWNLOAD_CACHE=$HOME/.pip/cache
```

You can set the path to anywhere you like (as long as you have write access). After adding this line, `source` your `.bashrc`{.interpreted-text role="file"} (or `.bash_profile`{.interpreted-text role="file"}) file and you will be all set.

Another way of doing the same configuration is via the `pip.conf`{.interpreted-text role="file"} or `pip.ini`{.interpreted-text role="file"} files, depending on your system. If you are on Windows, you can add the following line to your `pip.ini`{.interpreted-text role="file"} file under `[global]` settings:

``` console
download-cache = %USERPROFILE%\pip\cache
```

Similarly, on Unix systems you should simply add the following line to your `pip.conf`{.interpreted-text role="file"} file under `[global]` settings:

``` console
download-cache = $HOME/.pip/cache
```

Even though you can use any path you like to store your cache, it is recommended that you create a new folder *in* the folder where your `pip.conf`{.interpreted-text role="file"} or `pip.ini`{.interpreted-text role="file"} file lives. If you don"t trust yourself with all of this path voodoo, just use the values provided here and you will be fine.
