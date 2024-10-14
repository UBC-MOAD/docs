.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _HowToStructurePythonPackagesAndWhy:

****************************************
How to Structure Python Packages and Why
****************************************

In August 2019 Doug reviewed then-current opinions and recommended practices for Python packaging
and arrived at the a collection of recommendations for the directories and files layout of Python packages
developed by the UBC EOAS MOAD group.

Doug periodically reviews the Python packaging landscape and updates this section to keep pace
with changes in the packaging tools,
opinions,
and recommended practices.

The most recent review in the November 2022 to January 2023 time frame resulted in significant changes
to our package layout.
The `NEMO-Cmd`_ package was the first package that was converted from the previously used package layout to what is recommended here,
so its files are used as examples below.

.. _NEMO-Cmd: https://github.com/SalishSeaCast/NEMO-Cmd


References
==========

* `Hatch Documentation`_
* `Python Packaging User Guide`_
* `Hynek Schlawack's Testing & Packaging blog post`_
* `Ionel Cristian Mărieș's Packaging a python library blog post`_
* `Brian Skinn's My How and Why pyproject.toml & the 'src' Project Structure blog post`_
* `Brett Canon's Clarifying PEP 518 (a.k.a. pyproject.toml) blog post`_

.. _Hatch Documentation: https://hatch.pypa.io/latest/
.. _Python Packaging User Guide: https://packaging.python.org/en/latest/
.. _Hynek Schlawack's Testing & Packaging blog post: https://hynek.me/articles/testing-packaging/
.. _Ionel Cristian Mărieș's Packaging a python library blog post: https://blog.ionelmc.ro/2014/05/25/python-packaging/
.. _Brian Skinn's My How and Why pyproject.toml & the 'src' Project Structure blog post: https://bskinn.github.io/My-How-Why-Pyproject-Src/
.. _Brett Canon's Clarifying PEP 518 (a.k.a. pyproject.toml) blog post: https://snarky.ca/clarifying-pep-518/


.. note::
    There are 3 key things that people who are familiar with Python packaging need to understand about how the UBC EOAS MOAD group uses its internally developed packages:

    #. We rely heavily on `"Editable" Installs`_.
       That is,
       we install our packages from Git clones of the repositories using the :command:`python3 -m pip install --editable` command.
       That makes the workflow for getting updates into our installed packages a simple :command:`git pull` in the package repository clone directory.

       .. _"Editable" Installs: https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs

    #. On our local workstations and laptops we work in `conda environments`_,
       either the ``base`` environment created by
       :ref:`Installing Miniforge <InstallingMiniforge>`,
       or project-specific environments.

       .. _conda environments: https://docs.conda.io/projects/conda/en/latest/

    #. On HPC clusters we install our packages using the `"user scheme" for installation`_ in combination with `"Editable" Installs`_,
       that is,
       :command:`python3 -m pip install --user -e`.
       That enables job scripts running on compute nodes to find our package command-line interfaces without having to include the step of activating the environments in which the packages are installed.

       .. _"user scheme" for installation: https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site


Package Layout
==============

Using the `NEMO-Cmd`_ package as an example,
the directories and files layout of a MOAD package looks like:

.. code-block:: text

    NEMO-Cmd/
    ├── docs/
    │   ├── conf.py
    │   ├── index.rst
    │   ├── Makefile
    │   ├── ...
    │   ├── _static/
    │   │   └── ...
    ├── envs/
    │   ├── environment-dev.yaml
    │   ├── environment-rtd.yaml
    │   ├── environment-test.yaml
    │   └── requirements.txt
    ├── .github/
    │   ├── ...
    ├── .gitignore
    ├── LICENSE
    ├── nemo_cmd/
    │   ├── __about__.py
    │   ├── __init__.py
    │   ├── ...
    ├── .pre-commit-config.yaml
    ├── pyproject.toml
    ├── README.rst
    ├── .readthedocs.yaml
    └── tests/
        ├── ...

In summary
(please see the sections below for detail explanations):

* the :ref:`PkgTopLevelDirectory` is a clone of the package's Git repository.

It typically contains 6 files and 5 sub-directories.

The 6 files are described in the :ref:`PackageFiles` section below.

The 5 sub-directories in all packages are:

* :ref:`PkgPackageCodeSubDirectory` that contains the code modules
* :ref:`PkgDocsSubDirectory` that contains the `Sphinx`_ source files for the package documentation

  .. _Sphinx: https://www.sphinx-doc.org/en/master/

* :ref:`PkgEnvsSubDirectory` that contains the `conda environments`_ description YAML files for the package development and docs building environments,
* :ref:`PkgTestsSubDirectory` that contains the unit test suite for the package
* :ref:`PkgGithubSubDirectory` that contains configuration files for the GitHub Dependabot tool
  and GitHub Actions workflows that support repository management and QA on the package code and
  docs

The :file:`__about__.py` file in the :ref:`PkgPackageCodeSubDirectory` provides the package version identifier string as a variable named :py:obj:`__version__`.


.. _PkgTopLevelDirectory:

Top-Level Directory
-------------------

The name of the top-level directory is the "project name".
It does not have to be the same as the "package name" that you use in ``import`` statements.
In this example the "project name" is :file:`NEMO-Cmd`,
and the "package name" is ``nemo_cmd``.
Other examples of MOAD project and package names are:

* the :file:`moad_tools` package is named ``moad_tools``
* the :file:`Reshapr` package is named ``reshapr``
* the :file:`SalishSeaTools` package is named ``salishsea_tools``
* the :file:`SalishSeaNowcast` package is named ``nowcast``

The top-level directory "project name" is generally the name of the project's Git repository.


.. _PackageFiles:

Package Files
-------------

The sub-sections below describe the 6 files that are typically present in the top-level directory
of our packages.
Three of those files that *must* be present contain the information necessary to create a Python package:

* :ref:`PyprojectTomlFile` that contains the build system requirements and build backend tools to use
  for creation of the package,
  the package metadata,
  the command-line interface scripts and entry points configuration
  (if applicable),
  and configuration for tools used for code QA and package management
  (e.g. `coverage`_ and `hatch`_)

  .. _coverage: https://coverage.readthedocs.io/en/latest/
  .. _hatch: https://hatch.pypa.io/latest/

* :ref:`PkgReadmeRstFile` that provides the long description of the package
* :ref:`PkgLicenseFile` that contains the legal text of the Apache License, Version 2.0 license for the package

The other three files are perhaps optional,
but are present in most packages:

* :ref:`PkgReadthedocsYamlFile` that provides configuration for building the docs to the `readthedocs service`_
* :ref:`PkgGitignoreFile` that provides the list of intentionally untracked files that Git should ignore
* :ref:`PkgPreCommitConfigYamlFile` that provides configuration for the `pre-commit`_
  tool that is used to manage coding style and other aspects of repository QA

.. _readthedocs service: https://about.readthedocs.com/?ref=readthedocs.org
.. _pre-commit: https://pre-commit.com/


.. _PyprojectTomlFile:

:file:`pyproject.toml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`pyproject.toml` file contains:

* the build system requirements and build backend tools to use for creation of the package
* the package metadata
* the command-line interface scripts and entry points configuration (if applicable)
* configuration for tools used for code QA and package management (e.g. `coverage`_ and `hatch`_)

It is documented at https://hatch.pypa.io/latest/config/metadata/.

We use ``hatchling`` as our build backend,
so the ``build-system`` section of our :file:`pyproject.toml` files always looks like:

.. code-block:: toml

    [build-system]
    requires = ["hatchling"]
    build-backend = "hatchling.build"

The ``project`` section contains the essential metadata required to build the package.
Here is an example from the `NEMO-Cmd`_ package:

.. code-block:: toml

    [project]
    name = "NEMO-Cmd"
    dynamic = [ "version" ]
    description = "NEMO Command Processor"
    readme = "README.rst"
    requires-python = ">=3.10"
    license = { file = "LICENSE" }
    authors = [
        { name = "Doug Latornell", email = "dlatornell@eoas.ubc.ca" },
    ]
    keywords = ["automation", "oceanography", "ocean modelling", "UBC-MOAD"]
    classifiers = [
        "Development Status :: 5 - Production/Stable",
        "License :: OSI Approved :: Apache Software License",
        "Programming Language :: Python :: Implementation :: CPython",
        "Programming Language :: Python :: 3",
        "Programming Language :: Python :: 3.13",
        "Operating System :: POSIX :: Linux",
        "Operating System :: Unix",
        "Environment :: Console",
        "Intended Audience :: Science/Research",
        "Intended Audience :: Education",
        "Intended Audience :: Developers",
    ]
    dependencies = [
        # see envs/environment-dev.yaml for conda environment dev installation
        # see envs/requirements.txt for versions most recently used in development
        "arrow",
        "attrs",
        "cliff",
        "f90nml",
        "gitpython",
        "python-hglib",
        "pyyaml",
    ]

The ``project.urls`` section contains a table of important URLs for the package.
Not all of our packages have a change log,
but the URLs for documentation,
issue tracker,
and source code should exist and be included in this section.

.. code-block:: toml

    [project.urls]
    "Documentation" = "https://nemo-cmd.readthedocs.io/en/latest/"
    "Changelog" = "https://nemo-cmd.readthedocs.io/en/latest/CHANGES.html"
    "Issue Tracker" = "https://github.com/SalishSeaCast/NEMO-Cmd/issues"
    "Source Code" = "https://github.com/SalishSeaCast/NEMO-Cmd"

The optional ``project.scripts`` and  ``project.entry-points.plugin-namespace`` sections define the
package's command-line interface.

In the ``project.scripts`` section,
the key is the command name and its value is the code object that it will call.

.. code-block:: toml

    [project.scripts]
    nemo = "nemo_cmd.main:main"

The `NEMO-Cmd`_ package uses the `Cliff`_ package to define its CLI via entry points for each
of the sub-command plugins.
So,
its CLI definition also includes a ``project.entry-points.nemo`` section to connect the plugin
classes to the main `nemo` CLI script:

.. _Cliff: https://docs.openstack.org/cliff/latest/

.. code-block:: toml

    [project.entry-points.nemo]
    combine = "nemo_cmd.combine:Combine"
    deflate = "nemo_cmd.deflate:Deflate"
    gather = "nemo_cmd.gather:Gather"
    prepare = "nemo_cmd.prepare:Prepare"
    run = "nemo_cmd.run:Run"

Packages like `Reshapr`_ that use the `Click`_ package to define their CLI only require a
``project.scripts`` section because sub-commands are defined via Click.

.. _Reshapr: https://github.com/UBC-MOAD/Reshapr
.. _Click: https://click.palletsprojects.com/en/latest/

The ``tool.hatch.version`` section contains the path/file in which the package's version identifier
is stored.
This makes that file the "single source of truth" for the package's version,
facilitates management of the version identifier with the :command:`hatch version` command,
and enables the use of ``importlib.metadata.version()`` to access the package's version in code.

.. code-block:: toml

    [tool.hatch.version]
    path = "nemo_cmd/__about__.py"

The ``tool.coverage.run`` and ``tool.coverage.report`` sections provide configuration for the
`Coverage.py`_ tool that is used to monitor what lines of code the test suite exercises.

.. _Coverage.py: https://coverage.readthedocs.io/en/latest/

.. code-block:: toml

    [tool.coverage.run]
    branch = true
    source = [ "nemo_cmd", "tests"]

    [tool.coverage.report]
    show_missing = true


.. _PkgReadmeRstFile:

:file:`README.rst` File
^^^^^^^^^^^^^^^^^^^^^^^

The :file:`README.rst` file provides a full description of the package.
Take a look some of the UBC EOAS MOAD repositories to get an idea of typical contents.
:file:`README.rst` should include a copyright and license section.

The :file:`README.rst` file is included as the "long description" of the package via the

.. code-block:: toml

    readme = "README.rst"

line in the ``[project]`` section of the :ref:`PyprojectTomlFile`.

:file:`README` files written using reStructuredText
(or Markdown)
are automatically rendered to HTML in GitHub web pages.


.. _PkgLicenseFile:

:file:`LICENSE` File
^^^^^^^^^^^^^^^^^^^^

The :file:`LICENSE` contains the legal license text for the package.
We release all of our open code under the `Apache License, Version 2.0`_

.. _Apache License, Version 2.0: https://www.apache.org/licenses/

So,
you can just copy the :file:`LICENSE` file from another MOAD repository.
The :file:`LICENSE` file is included in the package metadata via the

.. code-block:: toml

    license = { file = "LICENSE" }

line in the ``[project]`` section of the :ref:`PyprojectTomlFile`.


.. _PkgReadthedocsYamlFile:

:file:`.readthedocs.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For packages that use the `readthedocs service`_ to render and host their documentation,
we include a :file:`.readthedocs.yaml` file in the top-level directory
(the file name and location are stipulated by readthedocs).
That file `declares the features of the environment`_ that we want readthedocs to use to build our docs,
specifically,
a conda environment that we describe in the :file:`envs/environment-rtd.yaml` file
(described below),
built using the ``mambaforge`` environment and package manager on an Ubuntu Linux virtual machine.

.. _declares the features of the environment: https://docs.readthedocs.io/en/stable/config-file/v2.html

The :file:`.readthedocs.yaml` file for the `NEMO-Cmd`_ package is typical,
and looks like:

.. code-block:: yaml

    version: 2

    build:
      os: ubuntu-22.04
      tools:
        python: "mambaforge-4.10"

    conda:
      environment: envs/environment-rtd.yaml

    # Only build HTML and JSON formats
    formats: []


.. _PkgGitignoreFile:

:file:`.gitignore` File
^^^^^^^^^^^^^^^^^^^^^^^

The :file:`.gitignore` file provides the list of intentionally untracked files that Git should ignore.
Having a comprehensive :file:`.gitignore` file makes commands like :command:`git status`,
:command:`git diff`,
:command:`git add`,
etc. easier to understand and use.

Our :file:`.gitignore` files are based on files generated by the PyCharm .ignore plugin.

The https://github.com/github/gitignore repository is also a good source for language-specific patterns
for :file:`.gitignore` files.


.. _PkgPreCommitConfigYamlFile:

:file:`.pre-commit-config.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`.pre-commit-config.yaml` file provides configuration for the `pre-commit`_ tool
that is used to manage coding style and other aspects of repository QA in many of our packages.
If it is used in a package you should be able to find notes about its use in the Coding Style
section of the package's development docs section;
e.g. `NEMO-Cmd Coding Style`_.

.. _NEMO-Cmd Coding Style: file:///media/doug/warehouse/MEOPAR/NEMO-Cmd/docs/_build/html/development.html#coding-style


Package Sub-Directories
-----------------------

The top-level directory must contain a package sub-directory in which the Python modules that are the package code are stored.
There are also usually 3 other sub-directories that contain:

* the package documentation (:file:`docs/`)
* descriptions of the conda environments used for development of the package and building its documentation (:file:`envs/`)
* the unit test suite for the package (:file:`tests/`)


.. _PkgPackageCodeSubDirectory:

Package Code Sub-directory
--------------------------

The package code sub-directory is where the Python modules that are the package code are stored.
Its name is the package name that is used in ``import`` statements.
In the the `NEMO-Cmd`_ package the package sub-directory is named :file:`nemo_cmd`.

Because the package name is used in ``import`` statements it must follow the rules that Python imposes on module names:

* contain only letters,
  numbers,
  and underscores
* not start with a number

By convention,
package sub-directory names are all-lowercase,
and use underscores when doing so improves readability.
A leading underscore is the convention that indicates a private module,
variable,
etc.,
so a package name that starts with an underscore would be unusual and confusing.

The package sub-directory must contain a file called :file:`__init__.py`
(often pronounced "dunder init").
The presence of a :file:`__init__.py` file is what makes a directory and the Python modules it contains importable.

In MOAD packages the :file:`__about__.py` file in the package sub-directory contains a declaration of a variable named :py:obj:`__version__`,
for example:

.. code-block:: python

    __version__ = "23.1.dev0"

We use a `CalVer`_ versioning scheme that conforms to `PEP-440`_.
The version identifier format is ``yy.n[.dev0]``,
where:

* ``yy`` is the (post-2000) year of release
* ``n`` is the number of the release within the year, starting at ``1``

After a release has been made the value of ``n`` is incremented by 1,
and ``.dev0`` is appended to the version identifier to indicate changes that will be included in the next release.

.. _CalVer: https://calver.org/
.. _PEP-440: https://peps.python.org/pep-0440/

The :py:obj:`__version__` value is included as the ``version`` metadata value in the :ref:`PyprojectTomlFile` by via the line:

.. code-block:: toml

    dynamic = [ "version" ]

in the ``[metadata]`` section,
and the ``tool.hatch.version`` section:

.. code-block:: toml

    [tool.hatch.version]
    path = "nemo_cmd/__about__.py"


.. _PkgDocsSubDirectory:

:file:`docs/` Sub-directory
---------------------------

The :file:`docs/` directory contains the `Sphinx`_ source files for the package documentation.
This directory is initialized by creating it,
then running the :command:`sphinx-quickstart` command in it.

After initializing the :file:`docs/` directory,
its :file:`conf.py` file requires some editing.
Please see :file:`docs/conf.py` in the `NEMO-Cmd`_ package for an example of a "finished" file.

The key things that need to be done are:

* Add:

  .. code-block:: python

      import os
      import sys

      sys.path.insert(0, os.path.abspath(".."))

  to the ``# -- Path setup ----------`` section of the file to make the package code
  directory tree available to the Sphinx builder for collection of package metadata,
  automatic generation of documentation from docstrings,
  etc.

* Change the :py:obj:`project` code in the ``# -- Project information ---------`` section to:

  .. code-block:: python

      import tomllib
      from pathlib import Path


      with Path("../pyproject.toml").open("rb") as f:
          pkg_info = tomllib.load(f)
      project = pkg_info["project"]["name"]

  to get the project name from the ``project`` section of the :ref:`PyprojectTomlFile`.

* Change the :py:obj:`copyright` code in the ``# -- Project information ---------`` section to something like:

  .. code-block:: python

      author = "SalishSeaCast Project Contributors and The University of British Columbia"
      pkg_creation_year = 2013
      copyright = f"{pkg_creation_year} – present, {author}"

  to ensure that the copyright year range displayed in the rendered docs ends with ``– present``
  and the copyright holder is correct.

* Change the :py:obj:`version` and :py:obj:`release` code in the
  ``# -- Project information ---------`` section to something like:

  .. code-block:: python

      import importlib.metadata

      version = importlib.metadata.version(project)
      release = version

  to get the package version identifier from the :py:obj:`__version__` variable in the package :file:`__about__.py` file.


.. _PkgEnvsSubDirectory:

:file:`envs/` Sub-directory
---------------------------

The :file:`envs/` sub-directory contains at least 4 files that describe the `conda environments`_
for the package development and docs building environments.


:file:`environment-dev.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`environment-dev.yaml` file is the conda environment description file for the package development environment.
It includes all of the packages necessary to install,
run,
develop,
test,
and document the package.

For example,
the :file:`environment-dev.yaml` file for the `NEMO-Cmd`_ package looks like:

.. code-block:: yaml

    # conda environment description file for NEMO-Cmd package
    # development environment
    #
    # Create a conda environment in which the `nemo` command is installed in editable mode
    # with:
    #
    #   $ conda env create -f NEMO-Cmd/envs/environment-dev.yaml
    #   $ conda activate nemo-cmd
    #
    # The environment includes all the tools used to develop,
    # test, and document the NEMO-Cmd package.
    #
    # See the envs/requirements.txt file for an exhaustive list of all the
    # packages installed in the environment and their versions used in
    # recent development.

    name: nemo-cmd

    channels:
      - conda-forge
      - nodefaults

    dependencies:
      - arrow
      - attrs
      - cliff
      - f90nml
      - gitpython
      - pip
      - python=3.13
      - pyyaml

      # For coding style, repo QA, and pkg management
      - black
      - hatch
      - pre-commit

      # For unit tests
      - pytest
      - pytest-cov
      - pytest-randomly

      # For documentation
      - sphinx
      - sphinx_rtd_theme
      - sphinx-notfound-page

      - pip:
        - python-hglib

        # install NEMO-Cmd package in editable mode
        - --editable ../

* The comments at the top of the file include a succinct version of the commands required to create the dev environment.
* The recommended conda channel to get packages from is ``conda-forge``.
  ``nodefaults`` is included in the ``channels`` list to speed up the packages dependency solver
  because it is now rare for us to require packages from any other source than ``conda-forge`` .
* Packages that are unavailable from conda channels are installed via :command:`pip`.

The :file:`environment-dev.yaml` file is "hand-crafted" rather than being generated via the :command:`conda env export` command.
As such,
it contains only the top level dependency packages,
and only version pins that are absolutely necessary.
That allows the conda solver do its job to assemble a consistent set of up-to-date packages to install.


:file:`environment-rtd.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`environment-rtd.yaml` file is the conda environment description file for the docs building environment on readthedocs.org.
It includes only the packages that are required to build the docs.

For example,
the :file:`environment-rtd.yaml` file for the `NEMO-Cmd`_ package looks like:

.. code-block:: yaml

    # conda environment description file for readthedocs build environment

    name: nemo-cmd-rtd

    channels:
      - conda-forge
      - nodefaults

    dependencies:
      - pip
      - python=3.13

      # RTD packages
      - mock
      - pillow
      - sphinx
      - sphinx_rtd_theme
      - sphinx-notfound-page

      - pip:
          # install package so that importlib.metadata functions can work
          - ../

The only reason to add more packages to the ``dependencies`` list is if :py:exc:`ImportError` exceptions
that arise in the `Sphinx autodoc`_ processing of docstrings can't be resolved by the use of the
`autodoc_mock_imports`_ list in :file:`conf.py`.

.. _Sphinx autodoc: https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
.. _autodoc_mock_imports: https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports


:file:`environment-test.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`environment-test.yaml` file is the conda environment description file for an environment configured
specifically for running the package's unit test suite,
and the :command:`sphinx linkcheck` command.
It is primarily used by GitHub Actions workflows that are run whenever commits are pushed to GitHub.
Please see the :ref:`PkgGitHubActionsWorkflows` section.


.. _RequirementsTxtFile:

:file:`requirements.txt` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`requirements.txt` file records the full list of packages in the development environment
and their versions used for recent development work.
It is generated using the :command:`python3 -m pip list --format=freeze` command.
When new package dependencies are added to the project,
or the dev environment is updated via :command:`conda env update --file envs/environment-dev.yaml`,
a new :file:`requirements.txt` file should be generated and merged with the previously committed version
so that the dev environment changes are tracked by Git.


.. _PkgTestsSubDirectory:

:file:`tests/` Sub-directory
----------------------------

The :file:`tests/` sub-directory contains the unit test suite for the package.
Its modules match the names of the modules in the :ref:`PkgPackageCodeSubDirectory`,
but with ``test_`` pre-pended to them.
If the :ref:`PkgPackageCodeSubDirectory` contains sub-directories,
those sub-directories are reflected in the :file:`tests/` tree.

The :file:`tests/` sub-directory,
nor any other directories that may be created in its tree *should not* contain :file:`__init__.py` files.
Please see `the discussion of test layout/import rules in the pytest docs`_ for explanation.

.. _the discussion of test layout/import rules in the pytest docs: https://doc.pytest.org/en/latest/explanation/goodpractices.html#tests-outside-application-code


.. _PkgGithubSubDirectory:

:file:`.github/` Sub-directory
------------------------------

The :file:`.github/` sub-directory contains files that configure automation on the GitHub servers
that supports repository QA.
In that sub-directory there is typically a :file:`dependabot.yaml` file and a :file:`workflows/` sub-directory.

The :file:`dependabot.yaml` file configures the `GitHub Dependabot`_ tool to check weekly
for version updates on GitHub actions packages used in the other automation workflows
and open pull requests to apply those updates.

.. _GitHub Dependabot: https://docs.github.com/en/code-security/dependabot


.. _PkgGitHubActionsWorkflows:

:file:`.github/workflows/` Sub-directory
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`workflows/` sub-directory contains configuration files for `GitHub Actions`_ workflows
used for code and docs QA tasks like:

.. _GitHub Actions: https://docs.github.com/en/actions

* static analysis of the code using `GitHub CodeQL`_ to detect possible security vulnerabilities

  .. _GitHub CodeQL: https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql

* run the package test suite with code coverage analysis (continuous integration)
  and pushing the coverage analysis report to `Codecov`/_

  .. _Codecov: https://about.codecov.io/

* run the `Sphinx linkcheck`_ builder on the documentation files to scans for external links,
  try to open them,
  and write report that highlights any links that are broken or redirected

  .. _Sphinx linkcheck: https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinx.builders.linkcheck.CheckExternalLinksBuilder

The workflows are generally run automatically each time that commits are pushed to the repository.
Some,
like the CodeQL security scan,
and the Sphinx link check are also run on a schedule to detect vulnerabilities and broken links that
arise between code and docs updates.

In most cases the workflows are based on our collection of shared reusable workflows in the `UBC-MOAD/gha-workflows`_ repository.

.. _UBC-MOAD/gha-workflows: https://github.com/UBC-MOAD/gha-workflows


Rationale
=========

This section explains the rationale for important choices in the packaging layout and methodology
described above.

The changes that resulted from Doug's December 2022 review of then current opinions and recommended practices for Python packaging are:

* Start using the :ref:`PyprojectTomlFile` in packages to contain all of the package metadata.
  That eliminates the :file:`setup.py` and :file:`setup.cfg` files.
  It also changes how the package name and version are accessed in the :file:`conf.py` file in the :ref:`PkgDocsSubDirectory`,
  and how the package version is accessed elsewhere in code
  (such as for the ``--version`` option in packages with command-line interfaces).

* Define the package version identifier in the :file:`__about__.py` file in the :ref:`PkgPackageCodeSubDirectory`.

* Change to use ``hatchling`` as our package build backend because it is more modern than ``setuptools``
  and provides additional package management tools like :command:`hatch version`.
  It also appears to continue to use the :file:`.pth` scheme for editable mode installs,
  in contrast to ``setuptools>=64.0.0`` possibility of using a custom import hook scheme that is not supported
  by IDEs like PyCharm and VS Code.

While the :file:`src/` layout advocated by `Hynek Schlawack's Testing & Packaging blog post`_
and `Ionel Cristian Mărieș's Packaging a python library blog post`_ is now also recommended
by the Python Packaging Authority in the `Python Packaging User Guide`_,
we have not yet adopted it.
The benefits that :file:`src/` layout provides are not important to us because we always install our
group-developed packages via :command:`python3 -m pip install -e`,
and we don't use `tox`_ to test our packages with different Python versions and interpreters.

.. _tox: https://tox.wiki/en/latest/
