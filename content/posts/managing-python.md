---
title: "Managing Python"
date: 2020-06-20T16:09:03-07:00
draft: false
toc: false
images:
tags:
  - python
  - programming
---

This weekend I was trying to set up Python on my personal machine. I found that managing Python on MacOS is somewhat of a nightmare. MacOS comes bundled with Python out of the box, which has been sufficient for me in the past. But what happens when you want to upgrade Python, hot swap Python versions, or manage your Python packages across projects? There are a few tools I'd recommend, starting with pyenv.

Pyenv allows you to manage multiple Python versions, both global to your system and/or local to your projects. Pyenv updates your system `PATH` to intercept all python-related commands you make (i.e. `python`, `python3`, `python3.8`) with a set of shim-binaries. These binaries call pyenv, which runs the command with the correct installation of Python.

First, install pyenv via [Brew](https://brew.sh/).
```
$ brew install pyenv
```
If you're running zsh (default on MacOS Catalina), then add the following line in your `~/.zshrc`. Otherwise, add it to your `~/.bashrc` / `config.fish` / etc.
```
# Configure the shell for pyenv. This will update PATH to point python
# requests to Pyenv shims.
if command -v pyenv 1>/dev/null 2>&1; then
  eval "$(pyenv init -)"
fi
```
Open a new terminal window or source your startup script from above. Now you can install Python using pyenv!
```
$ pyenv install --list # see available versions
$ pyenv install 3.8.0 # install Python 3.8.0
$ pyenv install 2.7.15 # install Python 2.7.15
$ pyenv global 3.8.0 # set global version
$ pyenv versions # check currently active version (should point to 3.8.0)
$ cd sample-project && pyenv local 2.7.15 # set the local Python version for a project
```

If you're using popular data science libraries like NumPy or scikit-learn, I'd also recommend installing Anaconda, which is an open-source package manager for scientific computing. Many people install Anaconda for access to software like Jupyter Notebooks.  There are many benefits to using Anaconda including:
- Version control for package installations
- Environment management
- Dependency management. Anaconda evaluates the dependencies of all currently installed packages when upgrading or installing new packages in order to avoid breaking upgrades.
- Your choice of a clean CLI or GUI

With Anaconda, you get access to `Conda`, which is the preferred utility to manage virtual environments. Virtual environments are powerful because they create isolated environments, each which maintains its own files, directories, and path. This makes it possible to manage dependencies and version requirements independently for each project. Installation is straightforward because their [docs are stellar](https://docs.anaconda.com/anaconda/install/mac-os/)[1].

When you install Anaconda, you get Jupyter Notebooks for free, so try spinning it up locally!
```
$ jupyter notebook
```
Be sure to run `conda init && conda config --set auto_activate_base False` so that there is no base Conda environment installed. This means that you can hop between using pyenv and Conda. If you want to create a virtual environment with Conda, it's straightforward:
```
$ conda create --name py38 python=3.8.0 # create an env 'py38' for version 3.8.0 (one-time)
$ conda activate py38 # activate the 'py38' environment
$ conda info # see info on currently active environment
$ conda env list # see all environment versions
$ conda deactivate # deactive the current environment
```
To create virtual environments with pyenv, you can use pyenv-virtualenv which is available through Brew. Its commands and purpose are similar to Conda. For now, I'm not using pyenv-virtualenv because my projects will primarily use packages from Anaconda.

As I start writing bigger projects, I'm excited to exercise the power of virtual environments. It's also shocking in hindsight that I was managing all Python dependencies globally in my system before I found these tools. Although managing Python on MacOS isn't intuitive, it's nice that there's tooling available to ease that burden.


[1] [Useful Conda cheat sheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)
