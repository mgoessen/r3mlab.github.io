---
layout: post
title: pip, virtualenv and pipenv cheatsheet
excerpt_separator:  <!--more-->
categories:
  - python
tags:
  - pip
  - virtualenv
  - pipenv
  - cheatsheet
---
A collection of useful commands to handle python virtual environments and packages with pip, virtualenv, and pipenv.

<!--more-->

## Pip
- ###### Global Help
```bash
pip help
```

- ###### Help for a specific command
```bash
pip help install
```

- ###### Search a package & show details
```bash
pip search <package>
pip show <package>
```

- ###### Install / Uninstall a package
```bash
pip install <package>
pip uninstall <package>
```

- ###### Install a package in a user space:
```bash
pip install <package> --user
```

- ###### Install a specific version of a package
```bash
pip install -I SomePackage1==1.1.0
# Several packages at once:
pip install -I SomePackage1==1.1.0 'SomePackage2>=1.0.4'
```

- ###### Show all installed packages & their versions
```bash
pip list
```

- ###### Show if new versions of the packages are available
```bash
pip list -o
pip list --outdated
```

- ###### Upgrade a package to the latest version:
```bash
pip install -U <package>
```

- ###### Upgrade all packages to their latest version:
```bash
pip list --outdated --format=freeze | grep -v '^\-e' | cut -d = -f 1  | xargs -n1 pip install -U
```

- ###### Write a list of requirements
```bash
pip freeze > requirements.txt
```

- ###### Install a list of requirements
```bash
pip install -r requirements.txt
```

## Virtualenv

- ###### Create a virtual environment
```bash
virtualenv -p /usr/bin/python2 .venv
virtualenv -p /usr/bin/python3 .venv
```

- ###### Activate/deactivate a virtualenv
```bash
source .venv/bin/activate
deactivate
```

## Pipenv

- ###### Install/Uninstall a package in a virtualenv
```bash
pipenv install <package>
# Note: If it doesn't exist, the virtual environment will be created.
pipenv uninstall <package>
```

- ###### Install a specific version of a package
```bash
pipenv install requests~=1.2
```
- ###### Update packages
```bash
pipenv update # updates all pkgs
pipenv update <package>
```
- ###### Activate/deactivate the environment
```bash
pipenv shell
exit # Not 'deactivate'!
```

- ###### Run a command in the environment
```bash
pipenv run <command>
```

- ###### Install a list of requirements
```bash
pipenv install -r requirements.txt
```

- ###### Export a list of requirements
```bash
pipenv lock -r > requirements.txt
```

- ###### Install a package as a dev-package
```bash
pipenv install <package> --dev
```

- ###### Recreate the environment with a different version of Python
```bash
pipenv --python 3.5
```

- ###### Remove a virtual environment (will not delete Pipfiles)
```bash
pipenv -rm
```

- ###### Install packages from a Pipfile
```bash
pip install
# In the same folder as the Pipfile
# Will create a virtualenv if it doesn't exist
```

- ###### Find out the location of the virtual environment for the project
```bash
pipenv --venv
```

- ###### Check for security vulnerabilities in installed packages
```bash
pipenv check
```

- ###### List packages and their dependencies
```bash
pipenv graph
```

- ###### Create or update the Pipfile.lock
```bash
pipenv lock
```

- ###### Create an environment using Pipfile.lock
```bash
pipenv install --ignore-pipfile
```

## Resources

- [Pip documentation](https://pip.pypa.io/en/stable/)
- [Virtualenv documentation](https://virtualenv.pypa.io/en/latest/)
- [Pipenv documentation](https://pipenv.readthedocs.io/en/latest/)
