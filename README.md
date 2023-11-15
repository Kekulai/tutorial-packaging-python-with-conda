# tutorial-packaging-python-with-conda
A consolidation of knowledge to demonstrate how to package a python project using conda that has requirements from conda



# Challenge Addressed

Current practice from conda-side is to mix pip and conda conventions, with conda documentation of build not updated, leading to difficulty in deploying a python packaging workflow in which the details of the conda/pip abstractions must be leaked through to the user.  This repo is an attempt to recover the minimal working model for both conda and setuptools to accomplish the following:
- package a python program with dependencies from conda
- use dependencies from private github repos
- setup and deploy a private conda channel



# Background Reading

[Understanding setup.py, setup.cfg and pyproject.toml in Python – SomeBeans](https://ianhopkinson.org.uk/2022/02/understanding-setup-py-setup-cfg-and-pyproject-toml-in-python/)


[A Practical Guide to Using Setup.py - GoDataDriven](https://godatadriven.com/blog/a-practical-guide-to-using-setup-py/)


# Example Workflows

1. Setup Code as follows:
  
```bash[^9]
mypackage
├── setup.py  # to maximize backwards compatability, I will not use pyproject.toml
|   # README.rst or README.md (a nice description of your package)
|   # LICENCE (properly chosen license information, e.g. MIT, BSD-3, GPL-3, MPL-2, etc...)
└── mypackage
    ├── __init__.py
    └── ... (other Python files)
```
[Quickstart - setuptools 68.2.2.post20231016 documentation](https://setuptools.pypa.io/en/latest/userguide/quickstart.html)

```bash
$ conda activate base
$ conda update conda
$ conda install conda-build 
$ git clone https://github.com/Kekulai/tutorial-packaging-python-with-conda.git
$ cd tutorial-packaging-python-with-conda
$ conda build -c conda-forge .
$ conda env create -n testmypackage
$ conda activate testmypackage
$ conda install --use-local -c conda-forge mypackage 
```

>Explicitly installing a local package bypasses the dependency resolver, as such the package's run dependencies will not be evaluated. See conda install --help or the install command reference page for more info.
[conda build docs](https://docs.conda.io/projects/conda-build/en/stable/user-guide/tutorials/build-pkgs.html)

```bash
$ python test.py
```
# Questions to Investigate
- [ ] Given the overlap between meta.yaml and setup.py, I am guessing once the environment is setup, setup.py can be empty for requirements, and I can go ahead and `pip install -e .` to develop
> When your project is installed, all of the dependencies not already installed will be located (via PyPI), downloaded, built (if necessary), and installed. This, of course, is a simplified scenario. You can also specify groups of extra dependencies that are not strictly required by your package to work, but that will provide additional functionalities. For more advanced use, see Dependencies Management in Setuptools.


# Important Details & Limitations

- conda packaging can not draw from pypi - [python - How do I install pip packages through a conda-build recipe? - Stack Overflow](https://stackoverflow.com/questions/64916092/how-do-i-install-pip-packages-through-a-conda-build-recipe)
- pip and setup.py can install from dependency links not from pypi -- Additionally how to install from private git repos are also described - [python - pip ignores dependency_links in setup.py - Stack Overflow](https://stackoverflow.com/questions/12518499/pip-ignores-dependency-links-in-setup-py)
- Best Practice for Conda development as of 202311 [What is the best workflow for developing python packages and publishing via Conda? · Issue #5058 · conda/conda-build](https://github.com/conda/conda-build/issues/5058)
  - Setup `pyproject.toml` or `setup.py`
  - Then create a meta.yaml conda package and manually find the conda equivalent of the deps
  - Install package in dev mode `pip install -e`
  - Build the package
  - Note that `conda develop` is not ready
-  [python - How do I install pip packages through a conda-build recipe? - Stack Overflow](https://stackoverflow.com/questions/64916092/how-do-i-install-pip-packages-through-a-conda-build-recipe)
- `Setup.py` is not needed for modern pip install, but is still required for build/publication - reference setuptools doc
- General confusion on why there so much overlap between meta.yaml and setup.py -- basically conda is for packing a wide variety of programs and pip bridges the gap for python specific needs, hence the overlap in information between meta.yaml and setup.py [[conda] meta.yaml versus setup.py](https://conda.continuum.narkive.com/cDWteatm/meta-yaml-versus-setup-py)
- to `conda build` with different channels you specify the channels needed in the command line execution not in the meta.yaml [Allow to specify channels in a recipe · Issue #532 · conda/conda-build](https://github.com/conda/conda-build/issues/532)
- `setup.py` is still the best way to do editable installation!
  > pip may allow editable install only with pyproject.toml and setup.cfg. However, this behavior may not be consistent over various pip versions and other packaging-related tools (setup.py is more reliable on those scenarios). [Configuring setuptools using pyproject.toml files - setuptools 68.2.2.post20231016 documentation](https://setuptools.pypa.io/en/latest/userguide/pyproject_config.html)
- more conda and pip nuance [Suggestions for a conda user preparing a package for PyPI - Questions about Python Packaging & Code - pyOpenSci Community Forum](https://pyopensci.discourse.group/t/suggestions-for-a-conda-user-preparing-a-package-for-pypi/324/2)



