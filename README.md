# tutorial-packaging-python-with-conda
A consolidation of knowledge to demonstrate how to package a python project using conda that has requirements from conda

# Challenge Addressed
Current practice from conda-side is to mix pip and conda conventions, with conda documentation of build not updated, leading to difficulty in deploying a python packaging workflow in which the details of the conda/pip abstractions must be leaked through to the user.  This repo is an attempt to recover the minimal working model for both conda and setuptools to accomplish the following:
- package a python program with dependencies from conda

# Limitations
- conda packaging draw from pypi[^1]

# Important Notes
- pip and setup.py installs from dependency links -- Additionally how to install from private git repos are also described [^2]
- 

[^1]: [python - How do I install pip packages through a conda-build recipe? - Stack Overflow](https://stackoverflow.com/questions/64916092/how-do-i-install-pip-packages-through-a-conda-build-recipe)
[^2]: [python - pip ignores dependency_links in setup.py - Stack Overflow](https://stackoverflow.com/questions/12518499/pip-ignores-dependency-links-in-setup-py)
