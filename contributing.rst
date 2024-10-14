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
.. image:: https://img.shields.io/badge/python-3.13-blue.svg
    :target: https://docs.python.org/3.13/
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
(the stuff after :command:`git clone` above)
from the :guilabel:`Code` button on the `repository`_ page.

.. note::

    The :command:`git clone` command above assumes that your are `connecting to GitHub using SSH`_.
    If it fails,
    please follow the instructions in our :ref:`SecureRemoteAccess` docs to set up your SSH keys and :ref:`CopyYourPublicSshKeyToGitHub`.

    .. _connecting to GitHub using SSH: https://docs.github.com/en/authentication/connecting-to-github-with-ssh


.. _MOAD-DocsBuildEnvironment:

Docs Build Environment
======================

.. image:: https://img.shields.io/badge/python-3.13-blue.svg
    :target: https://docs.python.org/3.13/
    :alt: Python Version

Setting up an isolated docs build environment using `Conda`_ is recommended.
Assuming that you have :ref:`Installed Miniforge <InstallingMiniforge>`,
you can create and activate an environment called ``moad-docs`` that will have all of the Python packages necessary for building the documentation with the commands:

.. _Conda: https://docs.conda.io/en/latest/

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
With your ``moad-docs`` environment activated,
use:

.. _reStructuredText: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _Sphinx: https://www.sphinx-doc.org/en/master/

.. code-block:: bash

    (moad-docs)$ make clean html

to do a clean build of the documentation.
The output looks something like:

.. code-block:: text

    Removing everything under '_build'...
    Running Sphinx v7.2.6
    making output directory... done
    loading intersphinx inventory from https://ubc-moad-tools.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://nemo-cmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishseacmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishsea-meopar-docs.readthedocs.io/en/latest/objects.inv...
    building [mo]: targets for 0 po files that are out of date
    writing output...
    building [html]: targets for 24 source files that are out of date
    updating environment: [new config] 24 added, 0 changed, 0 removed
    reading sources... [100%] zzz_archival_docs/index
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    copying assets... copying static files... done
    copying extra files... done
    done
    writing output... [100%] zzz_archival_docs/index
    generating indices... done
    copying linked files...
    copying notebooks ...
    writing additional pages... search done
    copying images... [100%] segrid_edit.png
    dumping search index in English (code: en)... done
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

    Removing everything under '_build'...
    Running Sphinx v5.0.2
    making output directory... done
    loading intersphinx inventory from https://ubc-moad-tools.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://nemo-cmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishseacmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishsea-meopar-docs.readthedocs.io/en/latest/objects.inv...
    building [mo]: targets for 0 po files that are out of date
    building [linkcheck]: targets for 22 source files that are out of date
    updating environment: [new config] 22 added, 0 changed, 0 removed
    reading sources... [100%] zzz_archival_docs/index
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    writing output... [100%] zzz_archival_docs/index

    (          ariane: line   37) -ignored- https://github.com/UBC-MOAD/ariane-2.3.0_03
    (python_packaging/pkg_structure: line   15) -ignored- https://github.com/SalishSeaCast/rpn-to-gemlam
    (      ssh_access: line   27) -ignored- https://www.baeldung.com/cs/ssh-intro
    (          xios-2: line   37) -ignored- https://github.com/SalishSeaCast/XIOS-2
    (          xios-2: line  751) -ignored- https://github.com/SalishSeaCast/NEMO-3.6-code
    (          ariane: line   23) ok        http://ariane.lagrangian.free.fr/ariane_install_2.x.x_sep08.pdf
    (          ariane: line   37) ok        http://ariane.lagrangian.free.fr/download.php
    (          ariane: line   25) ok        http://ariane.lagrangian.free.fr/ariane_tutorial_2.x.x_sep08.pdf
    (zzz_archival_docs/hg_version_control: line   27) ok        http://hgbook.red-bean.com/
    (zzz_archival_docs/hg_version_control: line   27) ok        http://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
    (          xios-2: line   15) ok        http://forge.ipsl.jussieu.fr/ioserver/wiki
    (          xios-2: line  933) ok        http://cfconventions.org/Data/cf-standard-names/29/build/cf-standard-name-table.html
    (zzz_archival_docs/hg_version_control: line   27) ok        http://hgbook.red-bean.com/read/how-did-we-get-here.html
    (alliance-computing: line   15) ok        https://alliancecan.ca/en
    (          xios-2: line  831) ok        http://forge.ipsl.jussieu.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf
    (alliance-computing: line   64) ok        https://alliancecan.ca/en/services/advanced-research-computing/account-management/apply-account
    (          ariane: line   24) ok        http://ariane.lagrangian.free.fr/ariane_namelist_2.x.x_oct08.pdf
    (git_version_control: line   60) ok        https://brew.sh/
    (          ariane: line   15) ok        http://ariane.lagrangian.free.fr/whatsariane.html
    (zzz_archival_docs/hg_version_control: line   41) ok        https://bitbucket.org/
    (python_packaging/pkg_structure: line   29) ok        https://bskinn.github.io/My-How-Why-Pyproject-Src/
    (python_packaging/pkg_structure: line   28) ok        https://blog.ionelmc.ro/2014/05/25/python-packaging/
    (          globus: line   25) ok        https://app.globus.org/file-manager
    (github_notebooks_readme: line    7) ok        https://commonmark.org/
    (    contributing: line   94) ok        https://conda.io/en/latest/
    (conda_pkg_env_mgr: line  203) ok        https://conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
    (    contributing: line   13) ok        https://creativecommons.org/licenses/by/4.0/
    (python_packaging/pkg_structure: line  405) ok        https://calver.org/
    (conda_pkg_env_mgr: line   42) ok        https://conda-forge.org/
    (alliance-computing: line   56) ok        https://ccdb.alliancecan.ca/account_application
    (   analysis_repo: line  138) ok        https://cookiecutter.readthedocs.io/en/latest/
    (python_packaging/pkg_structure: line  634) ok        https://doc.pytest.org/en/latest/explanation/goodpractices.html#tests-outside-application-code
    (         jupyter: line  353) ok        https://docs.alliancecan.ca/wiki/Anaconda/en
    (python_packaging/pkg_structure: line   53) ok        https://docs.conda.io/projects/conda/en/latest/
    (         jupyter: line  393) ok        https://docs.alliancecan.ca/wiki/Python#Creating_and_using_a_virtual_environment
    (      ssh_access: line  479) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
    (    contributing: line   78) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh
    (      ssh_access: line  386) ok        https://docs.alliancecan.ca/wiki/SSH_security_improvements#SSH_host_key_fingerprints
    (   analysis_repo: line  131) ok        https://docs.conda.io/en/latest/miniconda.html
    (conda_pkg_env_mgr: line   15) ok        https://docs.conda.io/en/latest/
    (    contributing: line   13) ok        https://docs.python.org/3.13/
    (git_version_control: line   22) ok        https://docs.github.com/en/get-started
    (      ssh_access: line   72) ok        https://docs.microsoft.com/en-ca/windows-server/administration/openssh/openssh_install_firstuse
    (          xios-2: line  708) ok        https://en.wikipedia.org/wiki/XML
    (     bash_config: line   43) ok        https://douglatornell.github.io/2013-09-26-ubc/lessons/ref/shell.html
    (    contributing: line  371) ok        https://git-scm.com/
    (git_version_control: line   22) ok        https://git-scm.com/book/en/v2
    (python_packaging/pkg_structure: line  267) ok        https://docs.openstack.org/cliff/latest/
    (git_version_control: line  213) ok        https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository
    (git_version_control: line  151) ok        https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config#ch_core_editor
    (git_version_control: line   22) ok        https://git-scm.com/doc
    (git_version_control: line  127) ok        https://git-scm.com/docs/git-config
    (python_packaging/pkg_structure: line  329) ok        https://docs.readthedocs.io/en/stable/config-file/v2.html
    ( getting_started: line   56) ok        https://github.com/
    (git_version_control: line   56) ok        https://git-scm.com/downloads
    (python_packaging/pkg_structure: line   31) ok        https://flit.pypa.io/en/latest/index.html
    (     sphinx_docs: line   36) ok        https://github.com/MIDOSS/docs
    (       vcs_repos: line   19) ok        https://github.com/MIDOSS/
    (       vcs_repos: line   18) ok        https://github.com/SalishSeaCast/
    (   analysis_repo: line   59) ok        https://github.com/SalishSeaCast
    (          xios-2: line  944) ok        https://github.com/SalishSeaCast/SS-run-sets/tree/main/v201702
    (     sphinx_docs: line   36) ok        https://github.com/SalishSeaCast/NEMO-Cmd
    (          xios-2: line  751) ok        https://github.com/SalishSeaCast/SS-run-sets
    (github_notebooks_readme: line    7) ok        https://github.com/SalishSeaCast/analysis-ben/tree/master/notebooks
    (          xios-2: line   37) ok        https://github.com/SalishSeaCast/XIOS-ARCH
    (     sphinx_docs: line   36) ok        https://github.com/SalishSeaCast/docs
    (   analysis_repo: line   32) ok        https://github.com/SalishSeaCast/analysis-susan
    ( getting_started: line   56) ok        https://github.com/UBC-MOAD
    (       vcs_repos: line   17) ok        https://github.com/UBC-MOAD/
    (   analysis_repo: line   50) ok        https://github.com/UBC-MOAD/cookiecutter-analysis-repo
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs
    (    contributing: line  397) ok        https://github.com/UBC-MOAD/docs/blob/main/CONTRIBUTORS.rst
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/workflows/sphinx-linkcheck/badge.svg
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/issues
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck
    (git_version_control: line  215) ok        https://github.com/github/gitignore/blob/main/Python.gitignore
    (     sphinx_docs: line   36) ok        https://github.com/UBC-MOAD/moad_tools
    (    contributing: line   13) ok        https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    (    contributing: line   13) ok        https://img.shields.io/badge/python-3.8+-blue.svg
    (    contributing: line   13) ok        https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    (oceanparcels/index: line   21) ok        https://gitter.im/OceanPARCELS/home
    (conda_pkg_env_mgr: line   42) ok        https://github.com/conda-forge/miniforge
    (         jupyter: line   24) ok        https://jupyter-notebook.readthedocs.io/en/stable/
    (         jupyter: line  422) ok        https://github.com/h5netcdf/h5netcdf
    (github_notebooks_readme: line   12) ok        https://jupyter.org/
    (github_notebooks_readme: line   12) ok        https://nbviewer.org/
    (    contributing: line   13) ok        https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    (      ssh_access: line  529) ok        https://linux.die.net/man/1/scp
    (python_packaging/pkg_structure: line   27) ok        https://hynek.me/articles/testing-packaging/
    (          xios-2: line   24) ok        https://nemo-cmd.readthedocs.io/en/latest/index.html#nemo-commandprocessor
    (          xios-2: line  772) ok        https://nemo-cmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (oceanparcels/index: line   13) ok        https://oceanparcels.org/index.html
    (oceanparcels/index: line   19) ok        https://oceanparcels.org/
    (oceanparcels/index: line   23) ok        https://oceanparcels.org/gh-pages/html/_modules/parcels/application_kernels/advection.html
    (python_packaging/pkg_structure: line   25) ok        https://packaging.python.org/en/latest/
    (python_packaging/pkg_structure: line   60) ok        https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site
    (python_packaging/pkg_structure: line  267) ok        https://palletsprojects.com/p/click/
    (python_packaging/pkg_structure: line  405) ok        https://peps.python.org/pep-0440/
    (python_packaging/pkg_structure: line  267) ok        https://packaging.python.org/en/latest/guides/creating-and-discovering-plugins/#using-package-metadata
    (python_packaging/pkg_structure: line   45) ok        https://pip.pypa.io/en/stable/cli/pip_install/#editable-installs
    (         jupyter: line  410) ok        https://pypi.org/
    (python_packaging/pkg_structure: line  657) ok        https://peps.python.org/pep-0518/
    (python_packaging/pkg_structure: line  148) ok        https://readthedocs.org/
    (oceanparcels/index: line   24) ok        https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/OceanParcelsRecipes.ipynb
    (oceanparcels/index: line   22) ok        https://os.copernicus.org/articles/17/1067/2021/
    (python_packaging/pkg_structure: line  111) ok        https://readthedocs.org
    (    contributing: line   46) ok        https://readthedocs.org/projects/ubc-moad-docs/builds/
    (     sphinx_docs: line  178) ok        https://rhodesmill.org/brandon/2012/one-sentence-per-line/
    (    contributing: line   13) ok        https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    (oceanparcels/index: line   20) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FC02ETTPHFPX
    (alliance-computing: line  271) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#compilenemo-3-6-graham
    (alliance-computing: line  260) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#createworkspaceandclonerepositories
    (alliance-computing: line  260) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#installcommandprocessorpackages
    (git_version_control: line  151) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FCFR6VU70S
    (          xios-2: line   24) ok        https://salishseacmd.readthedocs.io/en/latest/index.html#salishseacmdprocessor
    (python_packaging/pkg_structure: line  191) ok        https://setuptools.pypa.io/en/latest/userguide/declarative_config.html
    (          ariane: line  191) ok        https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/ParticleTracking.ipynb
    (          xios-2: line  772) ok        https://salishseacmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (python_packaging/pkg_structure: line  157) ok        https://setuptools.pypa.io/en/latest/build_meta.html
    (python_packaging/pkg_structure: line   26) ok        https://setuptools.pypa.io/en/latest/index.html
    (     bash_config: line   43) ok        https://software-carpentry.org/
    (zzz_archival_docs/hg_version_control: line   48) ok        https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/
    (python_packaging/pkg_structure: line   30) ok        https://snarky.ca/clarifying-pep-518/
    (zzz_archival_docs/hg_version_control: line   75) ok        https://tortoisehg.bitbucket.io/
    (    contributing: line   13) ok        https://ubc-moad-docs.readthedocs.io/en/latest/
    (     sphinx_docs: line   50) ok        https://ubc-moad-docs.readthedocs.io/en/latest/sphinx_docs.html#documentation-with-sphinx
    (python_packaging/pkg_structure: line  308) ok        https://www.apache.org/licenses/
    (    CONTRIBUTORS: line    7) ok        https://www.eoas.ubc.ca/~sallen/
    (     sphinx_docs: line   83) ok        https://ubc-moad-tools.readthedocs.io/en/latest/pkg_development.html#moad-toolspackageddevelopment
    (      ssh_access: line   32) ok        https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
    (python_packaging/pkg_structure: line  666) ok        https://tox.wiki/en/latest/
    (     sphinx_docs: line   19) ok        https://www.mathjax.org/
    (          globus: line   15) ok        https://www.globus.org/data-transfer
    (   analysis_repo: line  131) ok        https://www.anaconda.com/products/distribution
    (zzz_archival_docs/hg_version_control: line   63) ok        https://www.mercurial-scm.org/downloads
    (zzz_archival_docs/hg_version_control: line   21) ok        https://www.mercurial-scm.org/
    (zzz_archival_docs/hg_version_control: line   27) ok        https://www.mercurial-scm.org/wiki/BeginnersGuides
    (zzz_archival_docs/hg_version_control: line  247) ok        https://www.mercurial-scm.org/wiki/RebaseExtension
    (     sphinx_docs: line   19) ok        https://www.latex-project.org/
    (zzz_archival_docs/hg_version_control: line  247) ok        https://www.mercurial-scm.org/wiki/RebaseExtension#Scenario_A
    (zzz_archival_docs/hg_version_control: line  261) ok        https://www.mercurial-scm.org/wiki/RebaseExtension#Scenarios
    (zzz_archival_docs/hg_version_control: line   75) ok        https://www.sourcetreeapp.com/
    (    contributing: line  140) ok        https://www.sphinx-doc.org/en/master/
    (python_packaging/pkg_structure: line  605) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
    (     sphinx_docs: line  255) ok        https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html#module-sphinx.ext.intersphinx
    (python_packaging/pkg_structure: line  605) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports
    (    contributing: line  140) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
    (     sphinx_docs: line   57) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html
    (          xios-2: line  740) ok        https://www.xmlvalidation.com/
    (zzz_archival_docs/hg_version_control: line  207) ok        https://www.selenic.com/mercurial/hgignore.5.html
    (zzz_archival_docs/hg_version_control: line  170) ok        https://www.selenic.com/mercurial/hgrc.5.html
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
