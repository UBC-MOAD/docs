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

In August 2019 Doug reviewed current opinions and recommended practices for Python packaging and arrived at the recommendations below for the directories and files layout of Python packages developed by the UBC EOAS MOAD group.
The `rpn-to-gemlam tool`_ package was the first package that was converted from the previously used package layout to what is recommended here,
so its files are used as examples below.

.. _rpn-to-gemlam tool: https://github.com/SalishSeaCast/rpn-to-gemlam

Doug periodically reviews the Python packaging landscape and updates this section to keep pace
with changes in the packaging tools,
opinions,
and recommended practices.


References
==========

* `Python Packaging User Guide`_
* `Setuptools Documentation`_
* `Hynek Schlawack's Testing & Packaging blog post`_
* `Ionel Cristian Mărieș's Packaging a python library blog post`_
* `Brian Skinn's My How and Why pyproject.toml & the 'src' Project Structure blog post`_
* `Brett Canon's Clarifying PEP 518 (a.k.a. pyproject.toml) blog post`_
* `the Flit packaging and publisher tool`_

.. _Python Packaging User Guide: https://packaging.python.org/en/latest/
.. _Setuptools Documentation: https://setuptools.pypa.io/en/latest/index.html
.. _Hynek Schlawack's Testing & Packaging blog post: https://hynek.me/articles/testing-packaging/
.. _Ionel Cristian Mărieș's Packaging a python library blog post: https://blog.ionelmc.ro/2014/05/25/python-packaging/
.. _Brian Skinn's My How and Why pyproject.toml & the 'src' Project Structure blog post: https://bskinn.github.io/My-How-Why-Pyproject-Src/
.. _Brett Canon's Clarifying PEP 518 (a.k.a. pyproject.toml) blog post: https://snarky.ca/clarifying-pep-518/
.. _the Flit packaging and publisher tool: https://flit.pypa.io/en/latest/index.html


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

    #. On HPC clusters we use the system-provided Python 3 module and install our packages using the `"user scheme" for installation`_ in combination with `"Editable" Installs`_,
       that is,
       :command:`python3 -m pip install --user -e`.

       .. _"user scheme" for installation: https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site


Package Layout
==============

Using the `rpn-to-gemlam tool`_ package as an example,
the directories and files layout of a MOAD package looks like:

.. code-block:: text

    rpn-to-gemlam/
    ├── docs/
    │   ├── conf.py
    │   ├── index.rst
    │   ├── Makefile
    │   ├── ...
    │   └── _static/
    │       ├── ...
    ├── envs/
    │   ├── environment-dev.yaml
    │   ├── environment-rtd.yaml
    |   ├── requirements.txt
    ├── .readthedocs.yaml
    ├── LICENSE
    ├── pyproject.toml
    ├── README.rst
    ├── rpn_to_gemlam/
    │   ├── __init__.py
    │   ├── ...
    ├── setup.cfg
    └── tests/
        ├── ...

In summary
(please see the sections below for detail explanations):

* the :ref:`PkgTopLevelDirectory` is a clone of the package's Git repository.

It typically contains 5 files and 4 sub-directories.

The 5 files are:

* :ref:`PyprojectTomlFile` that contains the build system requirements and build backend tools to use for creation of the package
* :ref:`PkgSetupCfgFile` that contains the package metadata and ``setuptools`` options for creation of the package
* :ref:`PkgReadmeRstFile` that provides the long description of the package
* :ref:`PkgLicenseFile` that contains the legal text of the Apache License, Version 2.0 license for the package
* :ref:`PkgReadthedocsYmlFile` that provides configuration for building the docs to the https://readthedocs.org service

The 4 sub-directories are:

* :ref:`PkgPackageCodeSubDirectory` that contains the code modules
* :ref:`PkgDocsSubDirectory` that contains the `Sphinx`_ source files for the package documentation

  .. _Sphinx: https://www.sphinx-doc.org/en/master/

* :ref:`PkgEnvsSubDirectory` that contains the `conda environments`_ description YAML files for the package development and docs building environments,
* :ref:`PkgTestsSubDirectory` that contains the unit test suite for the package

The :file:`__init__.py` file in the :ref:`PkgPackageCodeSubDirectory` provides the package version identifier string as a variable named :py:obj:`__version__`.


.. _PkgTopLevelDirectory:

Top-Level Directory
-------------------

The name of the top-level directory is the "project name".
It does not have to be the same as the "package name" that you use in ``import`` statements.
In this example the "project name" is :file:`rpn-to-gemlam`,
and the "package name" is ``rpn_to_gemlam``.
Other examples of MOAD project and package names are:

* the :file:`moad_tools` package is named ``moad_tools``
* the :file:`SalishSeaTools` package is named ``salishsea_tools``
* the :file:`SalishSeaNowcast` package is named ``nowcast``

The top-level directory "project name" is generally the name of the project's Git repository,
however, keep in mind that Bitbucket converts repository names to all-lowercase.


Package Files
-------------

The top-level directory must contain 4 files that contain the information necessary to create a Python package.
It also contains a file to tell https://readthedocs.org/ how to configure an environment in which to build the package documentation.


.. _PyprojectTomlFile:

:file:`pyproject.toml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`pyproject.toml` file contains the build system requirements and build backend tools to use for creation of the package.
It is documented at https://setuptools.pypa.io/en/latest/build_meta.html.

We use ``setuptools`` as our build backend,
so our :file:`pyproject.toml` files always look like:

.. code-block:: toml

    [build-system]
    requires = ["setuptools", "wheel"]
    build-backend = "setuptools.build_meta"

.. warning::
    Editable installs of packages that contain a :file:`pyproject.toml` file are not supported by :command:`pip<21.3`
    (released 11-Oct-2021).
    At time of writing
    (29-Oct-2021)
    the Compute Canada clusters are using :command:`pip=20.0.2`.
    Until they upgrade to :command:`pip>=21.3` any of our packages that need to be installable on one of those clusters *cannot* contain a :file:`pyproject.toml` file.

    In place of the :file:`pyproject.toml` file,
    packages that have to be installed on Compute Canada clusters require a :file:`setup.py` file containing:

    .. code-block:: python

        import setuptools
        setuptools.setup()


.. _PkgSetupCfgFile:

:file:`setup.cfg` File
^^^^^^^^^^^^^^^^^^^^^^

The :file:`setup.cfg` file contains the package metadata and ``setuptools`` options for creation of the package.
It is documented at https://setuptools.pypa.io/en/latest/userguide/declarative_config.html.

A minimal :file:`setup.cfg` file looks like:

.. code-block:: ini

    [metadata]
    name = project_name
    version = 1.0
    description = One line description of the package
    author = your name
    auhor_email = your email address

    [options]
    zip_safe = False
    include_package_date = True
    packages = find:
    install_requires =
        list of packages that the package depends on, one per line

The :file:`setup.cfg` file for the `rpn-to-gemlam tool`_ package
(with the copyright header comment block excluded)
looks like:

.. code-block:: ini

    [metadata]
    name = rpn-to-gemlam
    version = attr: rpn_to_gemlam.__version__
    description = ECCC RPN to SalishSeaCast NEMO Atmospheric Forcing Conversion Tool
    author = Doug Latornell
    author_email = dlatornell@eoas.ubc.ca
    url=https://github.com/SalishSeaCast/rpn-to-gemlam
    long_description = file: README.rst
    license = Apache License, Version 2.0
    platform = Linux
    classifiers =
        Development Status :: 3 - Alpha
        License :: OSI Approved :: Apache Software License
        Programming Language :: Python :: Implementation :: CPython
        Programming Language :: Python :: 3
        Programming Language :: Python :: 3.6
        Programming Language :: Python :: 3.7
        Operating System :: POSIX :: Linux
        Operating System :: Unix
        Environment :: Console
        Intended Audience :: Science/Research
        Intended Audience :: Education

    [options]
    zip_safe = False
    include_package_data = True
    packages = find:
    python_requires = >=3.6
    install_requires =
        # see envs/environment-dev.yaml for conda environment dev installation
        # see envs/requirements.txt for versions most recently used in development
        angles
        arrow
        bottleneck
        Click
        matplotlib
        netCDF4
        python-dateutil
        pytz
        requests
        retrying
        scipy
        xarray
        # python3 -m pip install --editable ../tools/SalishSeaTools

    [options.entry_points]
    console_scripts =
        rpn-to-gemlam = rpn_to_gemlam.rpn_to_gemlam:cli

The ``[options.entry_points]`` stanza is an example of the declaration of `entry points`_.
They are used in packages that use a framework like `Click`_ or `Cliff`_ to provide a command-line interface.

.. _entry points: https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/#using-package-metadata
.. _Click: https://palletsprojects.com/p/click/
.. _Cliff: https://docs.openstack.org/cliff/latest/

.. note::
    Declaration of entry points in :file:`setup.cfg` is supported by ``setuptools>=51.0.0`` released on 6-Dec-2020.
    Any newly created conda environment will include a version of ``setuptools`` much newer than that.
    The same is true of the Compute Canada ``StdEnv/2020`` environment.
    The only platform where we can't support this feature is ``orcinus``.


.. _PkgReadmeRstFile:

:file:`README.rst` File
^^^^^^^^^^^^^^^^^^^^^^^

The :file:`README.rst` file provides a more than one line description of the package.
Take a look some of the UBC EOAS MOAD repositories to get an idea of typical contents.
:file:`README.rst` should include a copyright and license section.

The :file:`README.rst` file is included as the ``long_description`` metadata value in the :ref:`PkgSetupCfgFile` by including the line:

.. code-block:: ini

    long_description = file: README.rst

in the ``[metadata]`` section.

:file:`README` files written using reStructuredText
(or Markdown)
are automatically rendered to HTML in Bitbucket web pages.


.. _PkgLicenseFile:

:file:`LICENSE` File
^^^^^^^^^^^^^^^^^^^^

The :file:`LICENSE` contains the legal license text for the package.
We release all of our open code under the `Apache License, Version 2.0`_

.. _Apache License, Version 2.0: https://www.apache.org/licenses/

So,
you can just copy the :file:`LICENSE` file from another MOAD repository.
Be sure to include the license declaration via the ``license`` metadata value in the :ref:`PkgSetupCfgFile` by including the line:

.. code-block:: ini

    license = Apache License, Version 2.0

in the ``[metadata]`` section.


.. _PkgReadthedocsYmlFile:

:file:`.readthedocs.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

For packages that use https://readthedocs.org/ to render and host their documentation,
we include a :file:`.readthedocs.yaml` file in the top-level directory
(the file name and location are stipulated by readthedocs).
That file `declares the features of the environment`_ that we want readthedocs to use to build our docs,
specifically,
a conda environment that we describe in the :file:`envs/environment-rtd.yaml` file
(described below),
and the most recent version of Python.

.. _declares the features of the environment: https://docs.readthedocs.io/en/stable/config-file/v2.html

The :file:`.readthedocs.yaml` file for the `rpn-to-gemlam tool`_ package is typical,
and looks like:

.. code-block:: yaml

    version: 2

    build:
      os: ubuntu-20.04
      tools:
        python: "mambaforge-4.10"

    conda:
      environment: envs/environment-rtd.yaml

    # Only build HTML and JSON formats
    formats: []


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
In the the `rpn-to-gemlam tool`_ package the package sub-directory is named :file:`rpn_to_gemlam`.

Because the package name is used in ``import`` statements it must follow the rules that Python imposes on module names:

* contain only letters,
  numbers,
  and underscores
* not start with a number

By convention,
package names are all-lowercase,
and use underscores when they improve readability.
A leading underscore is the convention that indicates a private module,
variable,
etc.,
so a package name that starts with an underscore would be unusual and confusing.

The package sub-directory must contain a file called :file:`__init__.py`
(often pronounced "dunder init").
The presence of a :file:`__init__.py` file is what makes a directory and the Python modules it contains importable.

In MOAD packages the :file:`__init__.py` file in the package sub-directory contains a declaration of a variable named :py:obj:`__version__`,
for example:

.. code-block:: python

    __version__ = "19.1.dev0"

We use a `CalVer`_ versioning scheme that conforms to `PEP-440`_.
The version identifier format is ``yy.n[.devn]``,
where ``yy`` is the (post-2000) year of release,
and ``n`` is the number of the release within the year, starting at ``1``.
After a release has been made the value of ``n`` is incremented by 1,
and ``.dev0`` is appended to the version identifier to indicate changes that will be included in the next release.

.. _CalVer: https://calver.org/
.. _PEP-440: https://peps.python.org/pep-0440/

The :py:obj:`__version__` value is included as the ``version`` metadata value in the :ref:`PkgSetupCfgFile` by including the line:

.. code-block:: ini

    version = attr: package_name.__version__

in the ``[metadata]`` section.
Be sure to replace :py:obj:`package_name` with the package name you chose for the :ref:`PkgPackageCodeSubDirectory`.


.. _PkgDocsSubDirectory:

:file:`docs/` Sub-directory
---------------------------

The :file:`docs/` directory contains the `Sphinx`_ source files for the package documentation.
This directory is initialized by creating it,
then running the :command:`sphinx-quickstart` command in it.

After initializing the :file:`docs/` directory,
its :file:`conf.py` file requires some editing.
Please see :file:`docs/conf.py` in the `rpn-to-gemlam tool`_ package for an example of a "finished" file.

The key things that need to be done are:

* Add:

  .. code-block:: python

      import os
      import sys

      sys.path.insert(0, os.path.abspath(".."))

  to the ``# -- Path setup ----------`` section of the file to make the package code directory tree available to the Sphinx builder for collection of package metadata,
  automatic generation of documentation from docstrings,
  etc.

* Change the :py:obj:`project` code in the ``# -- Project information ---------`` section to:

  .. code-block:: python

      import configparser

      setup_cfg = configparser.ConfigParser()
      setup_cfg.read(os.path.abspath("../setup.cfg"))
      project = setup_cfg["metadata"]["name"]

  to get the project name from the ``metadata`` section of the :ref:`PkgSetupCfgFile`.

* Change the :py:obj:`copyright` code in the ``# -- Project information ---------`` section to something like:

  .. code-block:: python

      import datetime

      pkg_creation_year = 2019
      copyright_years = (
          f"{pkg_creation_year}"
          if datetime.date.today().year == pkg_creation_year
          else f"{pkg_creation_year}-{datetime.date.today():%Y}"
      )
      copyright = f"{copyright_years}, {author}"

  to ensure that the copyright year range displayed in the rendered docs is always up to date
  (at least as of the most recent rendering).

* Change the :py:obj:`version` and :py:obj:`release` code in the ``# -- Project information ---------`` section to something like:

  .. code-block:: python

      import package_name

      version = package_name.__version__
      release = version

  to get the package version identifier from the :py:obj:`__version__` variable in the package :file:`__init__.py` file.
  Be sure to replace :py:obj:`package_name` with the package name you chose for the :ref:`PkgPackageCodeSubDirectory`.


.. _PkgEnvsSubDirectory:

:file:`envs/` Sub-directory
---------------------------

The :file:`envs/` sub-directory contains at least 3 files that describe the `conda environments`_ for the package development and docs building environments.


:file:`environment-dev.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`environment-dev.yaml` file is the conda environment description file for the package development environment.
It includes all of the packages necessary to install,
run,
develop,
test,
and document the package.

For example,
the :file:`environment-dev.yaml` file for the `rpn-to-gemlam tool`_ package looks like:

.. code-block:: yaml

    # conda environment description file for rpn-to-gemlam package
    # development environment
    #
    # Create a conda environment for development, testing and documentation of the package
    # with:
    #
    #   $ conda env create -f rpn-to-gemlam/environment-dev.yaml
    #   $ conda activate rpn-to-gemlam
    #   (rpn-to-gemlam)$ python3 -m pip install --editable ../tools/SalishSeaTools
    #   (rpn-to-gemlam)$ python3 -m pip install --editable rpn-to-gemlam
    #
    # The environment will include all of the tools used to develop,
    # test, and document the rpn-to-gemlam package.
    #
    # See the envs/requirements.txt file for an exhaustive list of all of the
    # packages installed in the environment and their versions used in
    # recent development.

    name: rpn-to-gemlam

    channels:
      - conda-forge
      - nodefaults

    dependencies:
      - arrow
      - bottleneck
      - Click
      - matplotlib
      - netCDF4
      - pip
      - python=3.7
      - python-dateutil
      - pytz
      - requests
      - retrying
      - scipy
      - xarray

      # For unit tests
      - coverage
      - pytest

      # For documentation
      - sphinx
      - sphinx_rtd_theme

      # For coding style
      - black

      - pip:
          - angles

* The comments at the top of the file include a succinct version of the commands required to create the dev environment.
* The recommended conda channel to get packages from is ``conda-forge``.
  ``nodefaults`` is included in the ``channels`` list to speed up the packages dependency solver because it is now rare for us to require packages from any other source than ``conda-forge`` .
* Packages that are unavailable from conda channels are installed via :command:`pip`.

The :file:`environment-dev.yaml` file is "hand-crafted" rather than being generated via the :command:`conda env export` command.
As such,
it contains only the top level dependency packages,
and only version specifications that are absolutely necessary.
That allows the conda solver do its job to assemble a consistent set of up-to-date packages to install.


:file:`environment-rtd.yaml` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`environment-rtd.yaml` file is the conda environment description file for the docs building environment on readthedocs.org.
It includes only the packages above and beyond those that readthedocs.org installs into is environments as a matter of course that are required to build the docs.

The :file:`environment-rtd.yaml` file for the `rpn-to-gemlam tool`_ package is absolutely minimal,
specifying only the version of Python to use in the readthedocs.org environment:

.. code-block:: yaml

    # conda environment description file for docs build environment
    # on readthedocs.org

    name: sphinx-build

    channels:
      - defaults

    dependencies:
      - python=3.7

The only reason to add more packages to the ``dependencies`` list is if :py:exc:`ImportError` exceptions that arise in the `Sphinx autodoc`_ processing of docstrings can't be resolved by the use of the `autodoc_mock_imports`_ list in :file:`conf.py`.

.. _Sphinx autodoc: https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
.. _autodoc_mock_imports: https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports


.. _RequirementsTxtFile:

:file:`requirements.txt` File
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The :file:`requirements.txt` file records the full list of packages and their versions used for recent development work.
It is generated using the :command:`python3 -m pip list --format=freeze` command.
When new package dependencies are added to the project,
or the dev environment is updated via :command:`conda update --all`,
a new :file:`requirements.txt` file should be generated and merged with the previously committed version so that the dev environment changes are tracked by Git.


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


Rationale
=========

The changes that resulted from Doug's August 2019 review of then current opinions and recommended practices for Python packaging are:

* Start using the :ref:`PkgSetupCfgFile` in packages to contain all of the package metadata.
  That eliminates the :file:`__pkg_metadata__.py` that was previously used for some of the metadata,
  and was symlinked across the :ref:`PkgTopLevelDirectory` and :ref:`PkgPackageCodeSubDirectory`.
  It also dramatically reduces the amount of code in the :file:`setup.py`
  (for packages that still require it),
  and changes how the package name and version are imported into the :file:`conf.py` file in the :ref:`PkgDocsSubDirectory`.

* Define the package version identifier in the :file:`__init__.py` file in the :ref:`PkgPackageCodeSubDirectory`.

* Move the dev and docs environment description files in the :ref:`PkgEnvsSubDirectory`.

The :ref:`PkgSetupCfgFile` was chosen over the `pyproject.toml file`_ because,
as of ``pip-19.1`` in the spring of 2019,
`"Editable" Installs`_ are not supported for packages that contain a :file:`pyproject.toml` file.
Discussion by the Python Packaging Authority of how to resolve this issue is ongoing.

.. _pyproject.toml file: https://peps.python.org/pep-0518/

The :file:`src/` layout advocated by `Hynek Schlawack's Testing & Packaging blog post`_ and `Ionel Cristian Mărieș's Packaging a python library blog post`_ was rejected pending a strong recommendation in its favour by the Python Packaging Authority and support for it in packaging tools like `the Flit packaging and publisher tool`_.

The benefits that :file:`src/` layout provides are not important to us because always install our group-developed packages via :command:`python3 -m pip install -e`,
and we don't use `tox`_ to test our packages with different Python versions and interpreters.

.. _tox: https://tox.wiki/en/latest/
