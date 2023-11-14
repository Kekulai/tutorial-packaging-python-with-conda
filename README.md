# tutorial-packaging-python-with-conda
A consolidation of knowledge to demonstrate how to package a python project using conda that has requirements from conda

# Challenge Addressed

Current practice from conda-side is to mix pip and conda conventions, with conda documentation of build not updated, leading to difficulty in deploying a python packaging workflow in which the details of the conda/pip abstractions must be leaked through to the user.  This repo is an attempt to recover the minimal working model for both conda and setuptools to accomplish the following:
- package a python program with dependencies from conda
- use dependencies from private github repos
- setup and deploy a private conda channel

# Background Reading

[Understanding setup.py, setup.cfg and pyproject.toml in Python – SomeBeans](https://ianhopkinson.org.uk/2022/02/understanding-setup-py-setup-cfg-and-pyproject-toml-in-python/)

# Example Workflows

# Limitations

- conda packaging can not draw from pypi[^1]

# Important Details

- pip and setup.py can install from dependency links not from pypi -- Additionally how to install from private git repos are also described [^2]
- Best Practice for Conda development as of 202311[^3]
  - Setup `pyproject.toml` or `setup.py`
  - Then create a meta.yaml conda package and manually find the conda equivalent of the deps
  - Install package in dev mode `pip install -e`
  - Build the package
  - Note that `conda develop` is not ready
- `Setup.py` is not needed for modern pip install, but is still required for build/publication[^4]
- Conda can not build using dependencies from pypi -- you need to convert pypi packages to conda[^5]


[^1]: [python - How do I install pip packages through a conda-build recipe? - Stack Overflow](https://stackoverflow.com/questions/64916092/how-do-i-install-pip-packages-through-a-conda-build-recipe)
[^2]: [python - pip ignores dependency_links in setup.py - Stack Overflow](https://stackoverflow.com/questions/12518499/pip-ignores-dependency-links-in-setup-py)
[^3]: [What is the best workflow for developing python packages and publishing via Conda? · Issue #5058 · conda/conda-build](https://github.com/conda/conda-build/issues/5058)
[^4]: [Understanding setup.py, setup.cfg and pyproject.toml in Python – SomeBeans](https://ianhopkinson.org.uk/2022/02/understanding-setup-py-setup-cfg-and-pyproject-toml-in-python/)
[^5]: [python - How do I install pip packages through a conda-build recipe? - Stack Overflow](https://stackoverflow.com/questions/64916092/how-do-i-install-pip-packages-through-a-conda-build-recipe)
