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

First, install pyenv via Brew.
```
$ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)" # Installs Brew
$ brew install pyenv
```
If you're running zsh (default on MacOS Catalina), then add the following line in your ~/.zshrc. Otherwise, add it to your `~/.bashrc` / `config.fish` / etc.
```
## Initiate pyenv to manage the PATH environment variable in your terminal
#and insert the version of Python you want to run
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

If you're using popular data science libraries like NumPy or scikit-learn, I'd also recommend installing Anaconda, which is an open-source package manager for scientific computing. Many people install Anaconda for access to Jupyter Notebooks.  There are many benefits to using Anaconda including:
- Version Control for package installations
- Environment management
- Dependency management. Anaconda evaluates the dependencies of all currently installed packages when upgrading or installing new packages in order to avoid conflicts.
- Your choice of a clean CLI or GUI

Virtual environments are powerful because they create isolated environments to track dependencies. If you'd like to do something similar with pyenv, you can use pyenv-virtualenv which is available through Brew. Its commands and purpose are similar to Conda. Installation is straightforward because their [docs are stellar](https://docs.anaconda.com/anaconda/install/mac-os/)[1]. Be sure to run `conda init && conda config --set auto_activate_base False` so that there is no base Conda environment installed. This means that you can hop between using pyenv and Conda. If you want to enable Conda, it's straightforward:
```
$ conda create --name py38 python=3.8.0 # create an env 'py38' for version 3.8.0 (one-time)
$ conda activate py38 # activate the 'py38' environment
$ conda info # see info on currently active environment
$ conda env list # see all environment versions
$ conda deactivate # deactive the current environment
```
When you install Anaconda, you get Jupyter Notebooks for free, so try spinning it up locally!
```
$ jupyter notebook
```

[1] [Useful Conda cheat sheet](https://docs.conda.io/projects/conda/en/4.6.0/_downloads/52a95608c49671267e40c689e0bc00ca/conda-cheatsheet.pdf)
