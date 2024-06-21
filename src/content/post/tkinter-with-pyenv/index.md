---
title: "Using tkinter with pyenv"
description: "A Simple Two-Step Guide"
publishDate: "3 June 2023"
coverImage:
  src: "./cover.png"
  alt: "tkinter with pyenv cover image"
tags: ["python"]
---

I began a course taught by Christian Koch called "[Learn Python by Building 10 Apps with tkinter](https://www.udemy.com/course/learn-python-by-creating-10-apps/)". I soon realized I couldn't use `tkinter` even though it's a built-in Python module. Wait, what?

```sh frame="none"
Traceback (most recent call last):
  File "/Users/maxshapira/Development/public/tkinter-basics/main.py", line 1, in <module>
    import tkinter as tk
  File "/Users/maxshapira/.pyenv/versions/3.11.0/lib/python3.11/tkinter/__init__.py", line 38, in <module>
    import _tkinter # If this fails your Python may not be configured for Tk
    ^^^^^^^^^^^^^^^
ModuleNotFoundError: No module named '_tkinter'

```

Well, if you use `pyenv` to manage your Python versions (which you should), you'll run into this issue. I spent a lot of time trying to figure out the right solution, so I wanted to share it with you.

## Step 1: Installing Necessary System Packages

Using `tkinter` with `pyenv` can be tricky due to missing dependencies.

First, let's make sure we have installed the necessary system packages for `tkinter`.

In most Linux systems, you can install them with the following commands:

```sh frame="none"
sudo apt-get update
sudo apt-get install tk-dev
```

On macOS, you can use Homebrew to achieve the same:

```sh frame="none"
brew install tcl-tk
```

## Step 2: Linking the Correct Tcl/Tk Versions

Next, make sure you link the correct versions of Tcl/Tk when installing Python. `pyenv` builds Python in your environment, but if you don't have the required dependencies, like the Tk/Tcl libraries, it'll build Python without them.

If the Python version you want to use is already installed on your system, you'll need to uninstall it before proceeding. For example:

```sh frame="none"
pyenv uninstall 3.11.3
```

When installing a new Python version with `pyenv`, use the following commands:

For Linux:

```sh frame="none"
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev libbz2-dev \
libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev libncursesw5-dev \
xz-utils tk-dev libffi-dev liblzma-dev python-openssl git

env PYTHON_CONFIGURE_OPTS="--enable-shared" pyenv install <version>

```

For macOS, after installing tcl-tk with brew:

```sh frame="none"
brew install openssl readline sqlite3 xz zlib

env LDFLAGS="-L$(brew --prefix openssl@1.1)/lib -L$(brew --prefix readline)/lib -L$(brew --prefix sqlite3)/lib -L$(brew --prefix xz)/lib -L$(brew --prefix zlib)/lib -L$(brew --prefix tcl-tk)/lib" \
CPPFLAGS="-I$(brew --prefix openssl@1.1)/include -I$(brew --prefix readline)/include -I$(brew --prefix sqlite3)/include -I$(brew --prefix xz)/include -I$(brew --prefix zlib)/include -I$(brew --prefix tcl-tk)/include" \
PKG_CONFIG_PATH="$(brew --prefix openssl@1.1)/lib/pkgconfig:$(brew --prefix readline)/lib/pkgconfig:$(brew --prefix sqlite3)/lib/pkgconfig:$(brew --prefix xz)/lib/pkgconfig:$(brew --prefix zlib)/lib/pkgconfig:$(brew --prefix tcl-tk)/lib/pkgconfig" \
pyenv install <version>

```

Replace `<version>` with the version of Python you want to install. After that, you should be able to import `tkinter` in your `pyenv` Python environment.

These steps should help you fix the ﻿`tkinter` and ﻿`pyenv` issue.

### Note

I couldn't get ﻿`tkinter` to work with ﻿`pyenv` on Python ﻿`3.11.0`, but it worked perfectly on `3.11.2`.
