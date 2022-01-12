.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MOAD-DocsContributing:

**************************
Contributing to These Docs
**************************

.. image:: https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    :target: https://creativecommons.org/licenses/by/4.0/
    :alt: Licensed under the Creative Commons Attribution 4.0 International License
.. image:: https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    :target: https://github.com/UBC-MOAD/docs
    :alt: Git on GitHub
.. image:: https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    :target: https://ubc-moad-docs.readthedocs.io/en/latest/
    :alt: Documentation Status
.. image:: https://github.com/UBC-MOAD/docs/workflows/sphinx-linkcheck/badge.svg
    :target: https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck
    :alt: Sphinx linkcheck
.. image:: https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    :target: https://github.com/UBC-MOAD/docs/issues
    :alt: Issue Tracker
.. image:: https://img.shields.io/badge/python-3.8+-blue.svg
    :target: https://docs.python.org/3.10/
    :alt: Python Version

Additions,
improvements,
and corrections to these docs are *always* welcome.

The quickest way to fix typos, etc. on existing pages is to use the :guilabel:`Edit on GitHub` link in the upper right corner of the page to get to the online editor for the page on `GitHub`_.

For more substantial work,
and to add new pages,
the instructions below explain how to:

* clone the repository from `GitHub`_

* set up a conda environment in which you can build the docs locally instead of having to push commits to GitHub to trigger a `build on readthedocs.org`_

* build the docs with your changes,
  and preview them in Firefox

.. _GitHub: https://github.com/UBC-MOAD/docs
.. _build on readthedocs.org: https://readthedocs.org/projects/ubc-moad-docs/builds/


.. _MOAD-DocsGettingTheRepo:

Getting the Repo
================

.. image:: https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    :target: https://github.com/UBC-MOAD/docs
    :alt: Git on GitHub

Clone the MOAD documentation `repository`_ from GitHub with:

.. _repository: https://github.com/UBC-MOAD/docs

.. code-block:: bash

    $ git clone git@github.com:UBC-MOAD/docs.git

or copy the URI
(the stuff after :kbd:`git clone` above)
from the :guilabel:`Code` button on the `repository`_ page.

.. note::

    The :kbd:`git clone` command above assumes that your are `connecting to GitHub using SSH`_.
    If it fails,
    please follow the instructions in our :ref:`SecureRemoteAccess` docs to set up your SSH keys and :ref:`CopyYourPublicSshKeyToGitHub`.

    .. _connecting to GitHub using SSH: https://docs.github.com/en/authentication/connecting-to-github-with-ssh


.. _MOAD-DocsBuildEnvironment:

Docs Build Environment
======================

.. image:: https://img.shields.io/badge/python-3.8+-blue.svg
    :target: https://docs.python.org/3.10/
    :alt: Python Version

Setting up an isolated docs build environment using `Conda`_ is recommended.
Assuming that you have the `Anaconda Python Distribution`_ or `Miniconda3`_ installed,
you can create and activate an environment called :kbd:`moad-docs` that will have all of the Python packages necessary for building the documentation with the commands:

.. _Conda: https://conda.io/en/latest/
.. _Anaconda Python Distribution: https://www.anaconda.com/products/individual
.. _Miniconda3: https://docs.conda.io/en/latest/miniconda.html

.. code-block:: bash

    $ cd docs
    $ conda env create -f environment.yaml
    $ conda activate moad-docs

To deactivate the environment use:

.. code-block:: bash

    (moad-docs)$ conda deactivate

.. note::
    If you are using a version of :command:`conda` older than 4.4.0,
    the commands to activate and deactivate the environment are:

    .. code-block:: bash

        $ source activate moad-docs

    and

    .. code-block:: bash

        (moad-docs)$ source deactivate

    You can check what version of :command:`conda` you are using with :command:`conda --version`.


.. _MOAD-DocsBuildingAndPreviewingTheDocumentation:

Building and Previewing the Documentation
=========================================

.. image:: https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    :target: https://ubc-moad-docs.readthedocs.io/en/latest/
    :alt: Documentation Status

The MOAD documentation is written in `reStructuredText`_ and converted to HTML using `Sphinx`_.
Creating a :ref:`MOAD-DocsBuildEnvironment` as described above includes the installation of Sphinx.
Building the documentation is driven by the :file:`docs/Makefile`.
With your :kbd:`moad-docs` environment activated,
use:

.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Sphinx: https://www.sphinx-doc.org/en/master/

.. code-block:: bash

    (moad-docs)$ make clean html

to do a clean build of the documentation.
The output looks something like:

.. code-block:: text

    Removing everything under '_build'...
    Running Sphinx v1.7.1
    making output directory...
    loading pickled environment... not yet created
    loading intersphinx inventory from http://nemo-cmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from http://salishseacmd.readthedocs.io/en/latest/objects.inv...
    building [mo]: targets for 0 po files that are out of date
    building [html]: targets for 4 source files that are out of date
    updating environment: 4 added, 0 changed, 0 removed
    reading sources... [100%] xios-2looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    writing output... [100%] xios-2
    generating indices...
    writing additional pages... search
    copying static files... done
    copying extra files... done
    dumping search index in English (code: en) ... done
    dumping object inventory... done
    build succeeded.

    The HTML pages are in _build/html.

The HTML rendering of the docs ends up in :file:`docs/_build/html/`.
You can open the :file:`index.html` file in that directory tree in your browser to preview the results of the build.
To preview in Firefox from the command-line you can do:

.. code-block:: bash

    (moad-docs)$ firefox _build/html/index.html

If you have write access to the `repository`_ on GitHub,
whenever you push changes to GitHub the documentation is automatically re-built and rendered at https://ubc-moad-docs.readthedocs.io/en/latest/.


.. _MOAD-DocsLinkCheckingTheDocumentation:

Link Checking the Documentation
===============================

.. image:: https://github.com/UBC-MOAD/docs/workflows/sphinx-linkcheck/badge.svg
    :target: https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck
    :alt: Sphinx linkcheck

Use the commmand:

.. code-block:: bash

    (midoss-docs)$ make linkcheck

to check the documentation for broken links.
The output looks something like:

.. code-block:: text

    Running Sphinx v3.1.1
    loading pickled environment... done
    building [mo]: targets for 0 po files that are out of date
    building [linkcheck]: targets for 12 source files that are out of date
    updating environment: 0 added, 0 changed, 0 removed
    looking for now-outdated files... none found
    preparing documents... done
    writing output... [  8%] CONTRIBUTORS
    (line    7) ok        https://www.eoas.ubc.ca/~sallen/
    writing output... [ 16%] ariane
    (line   15) ok        http://stockage.univ-brest.fr/~grima/Ariane/whatsariane.html
    (line   37) -ignored- https://github.com/UBC-MOAD/ariane-2.2.8-code
    (line   37) ok        http://stockage.univ-brest.fr/~grima/Ariane/download.php
    (line  199) ok        https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/ParticleTracking.ipynb
    (line   23) ok        http://stockage.univ-brest.fr/~grima/Ariane/ariane_install_2.x.x_sep08.pdf
    (line   25) ok        http://stockage.univ-brest.fr/~grima/Ariane/ariane_tutorial_2.x.x_sep08.pdf
    (line   24) ok        http://stockage.univ-brest.fr/~grima/Ariane/ariane_namelist_2.x.x_oct08.pdf
    writing output... [ 25%] compute-canada
    (line   37) ok        https://www.westgrid.ca/
    (line  220) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/computecanada.html#createworkspaceandclonerepositories
    (line  220) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/computecanada.html#installcommandprocessorpackages
    (line  233) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/computecanada.html#compilenemo-3-6-computecanada
    (line   37) ok        https://www.computecanada.ca/
    (line   15) ok        https://www.computecanada.ca/
    (line   50) ok        https://www.computecanada.ca/research-portal/account-management/apply-for-an-account/
    (line   43) ok        https://ccdb.computecanada.ca/account_application
    writing output... [ 33%] contributing
    (line   13) ok        https://docs.python.org/3.10/
    (line   13) ok        https://creativecommons.org/licenses/by/4.0/
    (line   13) ok        https://ubc-moad-docs.readthedocs.io/en/latest/
    (line   13) ok        https://github.com/UBC-MOAD/docs/issues
    (line   35) ok        https://github.com/UBC-MOAD/docs
    (line   41) ok        https://github.com/UBC-MOAD/docs
    (line   13) ok        https://github.com/UBC-MOAD/docs
    (line   75) ok        https://help.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh
    (line   43) ok        https://readthedocs.org/projects/ubc-moad-docs/builds/
    (line  136) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
    (line  136) ok        https://www.sphinx-doc.org/en/master/
    (line   90) ok        https://docs.conda.io/en/latest/miniconda.html
    (line   55) ok        https://github.com/UBC-MOAD/docs
    (line   13) ok        https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    (line   90) ok        https://www.anaconda.com/products/individual
    (line  260) ok        https://git-scm.com/
    (line   90) ok        https://conda.io/en/latest/
    (line   13) ok        https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    (line   13) ok        https://img.shields.io/badge/python-3.8+-blue.svg
    (line   84) ok        https://img.shields.io/badge/python-3.8+-blue.svg
    (line   13) ok        https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    (line  286) ok        https://github.com/UBC-MOAD/docs/blob/master/CONTRIBUTORS.rst
    (line  130) ok        https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    (line   13) ok        https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    (line  268) ok        https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    writing output... [ 41%] getting_started
    (line   42) ok        https://github.com/
    (line   42) ok        https://github.com/UBC-MOAD
    writing output... [ 50%] globus
    (line   25) ok        https://app.globus.org/file-manager
    (line   15) ok        https://www.globus.org/data-transfer
    writing output... [ 58%] hg_version_control
    (line   13) ok        http://hgbook.red-bean.com/
    (line   13) ok        http://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
    (line   13) ok        http://hgbook.red-bean.com/read/how-did-we-get-here.html
    (line   13) ok        http://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
    (line   34) ok        https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/
    (line   31) ok        https://bitbucket.org/
    (line   27) ok        https://bitbucket.org/
    (line   61) ok        https://www.sourcetreeapp.com/
    (line   61) ok        https://tortoisehg.bitbucket.io/
    (line   49) ok        https://www.mercurial-scm.org/downloads
    (line    7) ok        https://www.mercurial-scm.org/
    (line   13) ok        https://www.mercurial-scm.org/wiki/BeginnersGuides
    (line  233) ok        https://www.mercurial-scm.org/wiki/RebaseExtension
    (line  233) ok        https://www.mercurial-scm.org/wiki/RebaseExtension#Scenario_A
    (line  247) ok        https://www.mercurial-scm.org/wiki/RebaseExtension#Scenarios
    (line  193) ok        https://www.selenic.com/mercurial/hgignore.5.html
    (line  156) ok        https://www.selenic.com/mercurial/hgrc.5.html
    writing output... [ 66%] index
    writing output... [ 75%] python_packaging/index
    writing output... [ 83%] python_packaging/pkg_structure
    (line   15) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line   26) ok        https://setuptools.readthedocs.io/en/latest/index.html
    (line   29) ok        https://bskinn.github.io/My-How-Why-Pyproject-Src/
    (line   25) ok        https://packaging.python.org/
    (line   28) ok        https://blog.ionelmc.ro/2014/05/25/python-packaging/
    (line   31) ok        https://flit.readthedocs.io/en/latest/index.html
    (line   45) ok        https://setuptools.readthedocs.io/en/latest/setuptools.html#development-mode
    (line   45) ok        https://pip.pypa.io/en/stable/reference/pip_install/#editable-installs
    (line   72) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line   62) ok        https://packaging.python.org/tutorials/installing-packages/#installing-to-the-user-site
    (line  112) ok        https://readthedocs.org
    (line   55) ok        https://docs.conda.io/projects/conda/en/latest/
    (line  159) ok        https://setuptools.readthedocs.io/en/latest/setuptools.html#configuring-setup-using-setup-cfg-files
    (line  180) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  252) ok        https://setuptools.readthedocs.io/en/latest/setuptools.html#dynamic-discovery-of-services-and-plugins
    (line  150) ok        https://readthedocs.org/
    (line  252) ok        https://click.palletsprojects.com/en/7.x/
    (line  258) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  122) ok        https://docs.conda.io/projects/conda/en/latest/
    (line  323) ok        https://docs.readthedocs.io/en/stable/config-file/v2.html
    (line  334) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  382) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line   27) ok        https://hynek.me/articles/testing-packaging/
    (line   30) ok        https://snarky.ca/clarifying-pep-518/
    (line  441) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  521) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  597) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (line  613) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
    (line  613) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports
    (line  412) ok        https://www.python.org/dev/peps/pep-0440
    (line  630) ok        https://doc.pytest.org/en/latest/goodpractices.html#tests-outside-application-code
    (line  661) ok        https://tox.readthedocs.io/en/latest/
    (line  652) ok        https://www.python.org/dev/peps/pep-0518/
    (line  412) ok        https://calver.org/
    (line  252) ok        https://docs.openstack.org/cliff/latest/
    (line  302) ok        https://www.apache.org/licenses/
    writing output... [ 91%] vcs_repos
    (line   19) ok        https://github.com/MIDOSS/
    (line   18) ok        https://github.com/SalishSeaCast/
    (line   17) ok        https://github.com/UBC-MOAD/
    writing output... [100%] xios-2
    (line   37) -ignored- https://github.com/SalishSeaCast/XIOS-2
    (line   24) ok        https://nemo-cmd.readthedocs.io/en/latest/index.html#nemo-commandprocessor
    (line   24) ok        https://salishseacmd.readthedocs.io/en/latest/index.html#salishseacmdprocessor
    (line  708) ok        https://en.wikipedia.org/wiki/XML
    (line  751) -ignored- https://github.com/SalishSeaCast/NEMO-3.6-code
    (line   37) ok        https://github.com/SalishSeaCast/XIOS-ARCH
    (line  772) ok        https://salishseacmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (line  772) ok        https://nemo-cmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (line  740) ok        https://www.xmlvalidation.com/
    (line  751) ok        https://github.com/SalishSeaCast/SS-run-sets
    (line   15) ok        http://forge.ipsl.jussieu.fr/ioserver/wiki
    (line   37) ok        http://forge.ipsl.jussieu.fr/ioserver/wiki
    (line  933) ok        http://cfconventions.org/Data/cf-standard-names/29/build/cf-standard-name-table.html
    (line 1054) ok        https://github.com/SalishSeaCast/SS-run-sets/tree/master/v201702
    (line  944) ok        https://github.com/SalishSeaCast/SS-run-sets/tree/master/v201702
    (line  902) ok        http://forge.ipsl.jussieu.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf
    (line  831) ok        http://forge.ipsl.jussieu.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf
    (line  959) ok        http://forge.ipsl.jussieu.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf

    build succeeded.

    Look for any errors in the above output or in _build/linkcheck/output.txt

:command:`make linkcheck` is run monthly via a `scheduled GitHub Actions workflow`_

.. _scheduled GitHub Actions workflow: https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck


.. _MOAD-DocsVersionControlRepository:

Version Control Repository
==========================

.. image:: https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    :target: https://github.com/UBC-MOAD/docs
    :alt: Git on GitHub

The MOAD documentation source files are available as a `Git`_ repository at https://github.com/UBC-MOAD/docs.

.. _Git: https://git-scm.com/


.. _MOAD-DocsIssueTracker:

Issue Tracker
=============

.. image:: https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    :target: https://github.com/UBC-MOAD/docs/issues
    :alt: Issue Tracker

Documentation tasks,
bug reports,
and enhancement ideas are recorded and managed in the issue tracker at https://github.com/UBC-MOAD/docs/issues.


License
=======

.. image:: https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    :target: https://creativecommons.org/licenses/by/4.0/
    :alt: Licensed under the Creative Commons Attribution 4.0 International License

The UBC EOAS MOAD Group Documentation is Copyright 2018 – present by by the `EOAS MOAD group`_ and The University of British Columbia.

.. _EOAS MOAD group: https://github.com/UBC-MOAD/docs/blob/main/CONTRIBUTORS.rst

It is licensed under a `Creative Commons Attribution 4.0 International License`_.

.. _Creative Commons Attribution 4.0 International License: https://creativecommons.org/licenses/by/4.0/
