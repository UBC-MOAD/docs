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
    Running Sphinx v7.2.6
    making output directory... done
    loading intersphinx inventory from https://ubc-moad-tools.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://nemo-cmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishseacmd.readthedocs.io/en/latest/objects.inv...
    loading intersphinx inventory from https://salishsea-meopar-docs.readthedocs.io/en/latest/objects.inv...
    building [mo]: targets for 0 po files that are out of date
    writing output...
    building [linkcheck]: targets for 24 source files that are out of date
    updating environment: [new config] 24 added, 0 changed, 0 removed
    reading sources... [100%] zzz_archival_docs/index
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    copying assets... done
    writing output... [100%] zzz_archival_docs/index

    (          ariane: line   37) -ignored- https://github.com/UBC-MOAD/ariane-2.3.0_03
    (      ssh_access: line   26) -ignored- https://www.baeldung.com/cs/ssh-intro
    (      ssh_access: line  556) -ignored- https://linux.die.net/man/1/scp
    (          xios-2: line   41) -ignored- https://github.com/SalishSeaCast/XIOS-2
    (          xios-2: line  229) -ignored- https://github.com/SalishSeaCast/NEMO-3.6-code
    (          ariane: line   15) ok        http://ariane.lagrangian.free.fr/whatsariane.html
    (          ariane: line   23) ok        http://ariane.lagrangian.free.fr/ariane_install_2.x.x_sep08.pdf
    (          ariane: line   37) ok        http://ariane.lagrangian.free.fr/download.php
    (          ariane: line   25) ok        http://ariane.lagrangian.free.fr/ariane_tutorial_2.x.x_sep08.pdf
    (          ariane: line   24) ok        http://ariane.lagrangian.free.fr/ariane_namelist_2.x.x_oct08.pdf
    (          xios-2: line  423) ok        http://cfconventions.org/Data/cf-standard-names/29/build/cf-standard-name-table.html
    (python_packaging/pkg_structure: line  181) ok        https://about.readthedocs.com/?ref=readthedocs.org
    (alliance-computing: line   15) ok        https://alliancecan.ca/en
    (alliance-computing: line   64) ok        https://alliancecan.ca/en/services/advanced-research-computing/account-management/apply-account
    (python_packaging/pkg_structure: line   38) ok        https://blog.ionelmc.ro/2014/05/25/python-packaging/
    (python_packaging/pkg_structure: line   39) ok        https://bskinn.github.io/My-How-Why-Pyproject-Src/
    (git_version_control: line   60) ok        https://brew.sh/
    (          globus: line   25) ok        https://app.globus.org/file-manager
    (zzz_archival_docs/hg_version_control: line   41) ok        https://bitbucket.org/
    (python_packaging/pkg_structure: line  296) ok        https://click.palletsprojects.com/en/latest/
    (github_notebooks_readme: line    7) ok        https://commonmark.org/
    (python_packaging/pkg_structure: line  483) ok        https://calver.org/
    (   analysis_repo: line  156) ok        https://cookiecutter.readthedocs.io/en/latest/
    (alliance-computing: line   56) ok        https://ccdb.alliancecan.ca/account_application
    (    contributing: line   13) ok        https://creativecommons.org/licenses/by/4.0/
    (python_packaging/pkg_structure: line  164) ok        https://coverage.readthedocs.io/en/latest/
    (python_packaging/pkg_structure: line  745) ok        https://doc.pytest.org/en/latest/explanation/goodpractices.html#tests-outside-application-code
    (conda_pkg_env_mgr: line   39) ok        https://conda-forge.org/
    (conda_pkg_env_mgr: line   15) ok        https://docs.conda.io/en/latest/
    (python_packaging/pkg_structure: line   60) ok        https://docs.conda.io/projects/conda/en/latest/
    (          vscode: line   15) ok        https://code.visualstudio.com/
    (         jupyter: line  354) ok        https://docs.alliancecan.ca/wiki/Anaconda/en
    (conda_pkg_env_mgr: line  206) ok        https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
    (    contributing: line   78) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh
    (python_packaging/pkg_structure: line  773) ok        https://docs.github.com/en/actions
    (python_packaging/pkg_structure: line  778) ok        https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql
    (python_packaging/pkg_structure: line  761) ok        https://docs.github.com/en/code-security/dependabot
    (git_version_control: line   22) ok        https://docs.github.com/en/get-started
    (alliance-computing: line  121) ok        https://docs.alliancecan.ca/wiki/SSH_Keys#Using_CCDB
    (      ssh_access: line  506) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
    (oceanparcels/index: line   23) ok        https://docs.oceanparcels.org/en/latest/reference/predefined_kernels.html
    (python_packaging/pkg_structure: line  376) ok        https://docs.readthedocs.io/en/stable/config-file/v2.html
    (    contributing: line   13) ok        https://docs.python.org/3.13/
    (     bash_config: line   43) ok        https://douglatornell.github.io/2013-09-26-ubc/lessons/ref/shell.html
    (python_packaging/pkg_structure: line  279) ok        https://docs.openstack.org/cliff/latest/
    (          xios-2: line  179) ok        https://en.wikipedia.org/wiki/XML
    (    contributing: line  405) ok        https://git-scm.com/
    (          vscode: line  149) ok        https://fortls.fortran-lang.org/index.html
    (git_version_control: line   22) ok        https://git-scm.com/book/en/v2
    (git_version_control: line  213) ok        https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository
    (git_version_control: line  151) ok        https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config#ch_core_editor
    (git_version_control: line   22) ok        https://git-scm.com/doc
    (git_version_control: line   56) ok        https://git-scm.com/downloads
    (git_version_control: line  127) ok        https://git-scm.com/docs/git-config
    ( getting_started: line   55) ok        https://github.com/
    (       vcs_repos: line   19) ok        https://github.com/MIDOSS/
    (          xios-2: line  315) ok        https://forge.ipsl.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf
    (     sphinx_docs: line   36) ok        https://github.com/MIDOSS/docs
    (          xios-2: line   15) ok        https://forge.ipsl.fr/ioserver/wiki
    (oceanparcels/index: line   21) ok        https://github.com/OceanParcels/parcels/discussions
    (   analysis_repo: line   62) ok        https://github.com/SalishSeaCast
    (       vcs_repos: line   18) ok        https://github.com/SalishSeaCast/
    (          xios-2: line  235) ok        https://github.com/SalishSeaCast/SS-run-sets
    (          xios-2: line  435) ok        https://github.com/SalishSeaCast/SS-run-sets/tree/main/v201702
    (python_packaging/pkg_structure: line   24) ok        https://github.com/SalishSeaCast/NEMO-Cmd
    (          xios-2: line   41) ok        https://github.com/SalishSeaCast/XIOS-ARCH
    (github_notebooks_readme: line    7) ok        https://github.com/SalishSeaCast/analysis-ben/tree/master/notebooks
    (     sphinx_docs: line   36) ok        https://github.com/SalishSeaCast/docs
    (   analysis_repo: line   33) ok        https://github.com/SalishSeaCast/analysis-susan
    (   analysis_repo: line  316) ok        https://github.com/SalishSeaCast/tools
    (       vcs_repos: line   17) ok        https://github.com/UBC-MOAD/
    ( getting_started: line   55) ok        https://github.com/UBC-MOAD
    (         jupyter: line  394) ok        https://docs.alliancecan.ca/wiki/Python#Creating_and_using_a_virtual_environment
    (     sphinx_docs: line   79) ok        https://github.com/UBC-MOAD/PythonNotes/blob/main/sphinx-docs/SphinxDocsTutorial.ipynb
    (conda_pkg_env_mgr: line   31) ok        https://github.com/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb
    (python_packaging/pkg_structure: line  296) ok        https://github.com/UBC-MOAD/Reshapr
    (   analysis_repo: line   51) ok        https://github.com/UBC-MOAD/cookiecutter-analysis-repo
    (    contributing: line  431) ok        https://github.com/UBC-MOAD/docs/blob/main/CONTRIBUTORS.rst
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/issues
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/workflows/sphinx-linkcheck/badge.svg
    (python_packaging/pkg_structure: line  799) ok        https://github.com/UBC-MOAD/gha-workflows
    (conda_pkg_env_mgr: line   39) ok        https://github.com/conda-forge/miniforge
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck
    (     sphinx_docs: line   36) ok        https://github.com/UBC-MOAD/moad_tools
    (python_packaging/pkg_structure: line   35) ok        https://hatch.pypa.io/latest/
    (python_packaging/pkg_structure: line  202) ok        https://hatch.pypa.io/latest/config/metadata/
    (python_packaging/pkg_structure: line  419) ok        https://github.com/github/gitignore
    (git_version_control: line  215) ok        https://github.com/github/gitignore/blob/main/Python.gitignore
    (         jupyter: line  423) ok        https://github.com/h5netcdf/h5netcdf
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/read/how-did-we-get-here.html
    (    contributing: line   13) ok        https://img.shields.io/badge/python-3.13-blue.svg
    (    contributing: line   13) ok        https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    (    contributing: line   13) ok        https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    (github_notebooks_readme: line   12) ok        https://jupyter.org/
    (      ssh_access: line   71) ok        https://learn.microsoft.com/en-ca/windows-server/administration/openssh/openssh_install_firstuse
    (         jupyter: line   24) ok        https://jupyter-notebook.readthedocs.io/en/stable/
    (python_packaging/pkg_structure: line   37) ok        https://hynek.me/articles/testing-packaging/
    (          vscode: line  121) ok        https://marketplace.visualstudio.com/items?itemName=fortran-lang.linter-gfortran
    (          vscode: line  115) ok        https://marketplace.visualstudio.com/items?itemName=Digoro.Clipboard
    (    contributing: line   13) ok        https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    (          vscode: line  103) ok        https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens
    (          vscode: line  128) ok        https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv
    (          vscode: line   88) ok        https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter-renderers
    (          vscode: line   97) ok        https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh
    (          vscode: line  109) ok        https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
    (          vscode: line  134) ok        https://marketplace.visualstudio.com/items?itemName=trond-snekvik.simple-rst
    (          vscode: line  140) ok        https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf
    (          ariane: line  787) ok        https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_TimeRes.ipynb
    (github_notebooks_readme: line   12) ok        https://nbviewer.org/
    (          ariane: line  560) ok        https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_Tracers.ipynb
    (oceanparcels/index: line   24) ok        https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/OceanParcelsRecipes.ipynb
    (          ariane: line  195) ok        https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/ParticleTracking.ipynb
    (conda_pkg_env_mgr: line   30) ok        https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb
    (     sphinx_docs: line   78) ok        https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/sphinx-docs/SphinxDocsTutorial.ipynb
    (          xios-2: line   26) ok        https://nemo-cmd.readthedocs.io/en/latest/index.html#nemo-commandprocessor
    (          xios-2: line  255) ok        https://nemo-cmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (oceanparcels/index: line   19) ok        https://oceanparcels.org/
    (          python: line   20) ok        https://numpy.org/
    (          vscode: line   82) ok        https://marketplace.visualstudio.com/items?itemName=ms-python.python
    (python_packaging/pkg_structure: line   36) ok        https://packaging.python.org/en/latest/
    (oceanparcels/index: line   13) ok        https://oceanparcels.org/index.html
    (python_packaging/pkg_structure: line   67) ok        https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site
    (python_packaging/pkg_structure: line  483) ok        https://peps.python.org/pep-0440/
    (python_packaging/pkg_structure: line   53) ok        https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs
    (         jupyter: line  411) ok        https://pypi.org/
    (          python: line   20) ok        https://pandas.pydata.org/
    (python_packaging/pkg_structure: line  183) ok        https://pre-commit.com/
    (oceanparcels/index: line   22) ok        https://os.copernicus.org/articles/17/1067/2021/
    (          python: line   51) ok        https://realpython.com/
    (    contributing: line   13) ok        https://readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    (     sphinx_docs: line  183) ok        https://rhodesmill.org/brandon/2012/one-sentence-per-line/
    (    contributing: line   46) ok        https://readthedocs.org/projects/ubc-moad-docs/builds/
    (alliance-computing: line  242) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#compilenemo-3-6-graham
    (alliance-computing: line  236) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#installcommandprocessorpackages
    (alliance-computing: line  232) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/graham.html#createworkspaceandclonerepositories
    (   analysis_repo: line  320) ok        https://salishsea-meopar-tools.readthedocs.io/en/latest/visualisation/visualization_workflows_xarray.html
    (   analysis_repo: line  320) ok        https://salishsea-meopar-tools.readthedocs.io/en/latest/SalishSeaTools/index.html
    (alliance-computing: line  236) ok        https://salishseacmd.readthedocs.io/en/latest/index.html#salishseacmdprocessor
    (          xios-2: line  255) ok        https://salishseacmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (python_packaging/pkg_structure: line   40) ok        https://snarky.ca/clarifying-pep-518/
    (     bash_config: line   43) ok        https://software-carpentry.org/
    (git_version_control: line  151) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FCFR6VU70S
    (oceanparcels/index: line   20) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FC02ETTPHFPX
    (python_packaging/pkg_structure: line  826) ok        https://tox.wiki/en/latest/
    (    contributing: line   13) ok        https://ubc-moad-docs.readthedocs.io/en/latest/
    (     sphinx_docs: line   50) ok        https://ubc-moad-docs.readthedocs.io/en/latest/sphinx_docs.html#documentation-with-sphinx
    (zzz_archival_docs/hg_version_control: line   75) ok        https://tortoisehg.bitbucket.io/
    (     sphinx_docs: line   88) ok        https://ubc-moad-tools.readthedocs.io/en/latest/pkg_development.html#moad-toolspackageddevelopment
    (zzz_archival_docs/hg_version_control: line   48) ok        https://support.atlassian.com/bitbucket-cloud/docs/configure-ssh-and-two-step-verification/
    (zzz_archival_docs/hg_version_control: line   27) ok        https://wiki.mercurial-scm.org/BeginnersGuides
    (conda_pkg_env_mgr: line   15) ok        https://www.anaconda.com/download
    (python_packaging/pkg_structure: line  355) ok        https://www.apache.org/licenses/
    (      ssh_access: line   31) ok        https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
    (zzz_archival_docs/hg_version_control: line  248) ok        https://wiki.mercurial-scm.org/RebaseExtension#Scenario_A
    (    CONTRIBUTORS: line    7) ok        https://www.eoas.ubc.ca/~sallen/
    (zzz_archival_docs/hg_version_control: line  248) ok        https://wiki.mercurial-scm.org/RebaseExtension
    (zzz_archival_docs/hg_version_control: line  262) ok        https://wiki.mercurial-scm.org/RebaseExtension#Scenarios
    (     sphinx_docs: line   19) ok        https://www.mathjax.org/
    (zzz_archival_docs/hg_version_control: line   21) ok        https://www.mercurial-scm.org/
    (zzz_archival_docs/hg_version_control: line   63) ok        https://www.mercurial-scm.org/downloads
    (          python: line   15) ok        https://www.python.org/
    (          globus: line   15) ok        https://www.globus.org/data-transfer
    (zzz_archival_docs/hg_version_control: line   75) ok        https://www.sourcetreeapp.com/
    (     sphinx_docs: line   19) ok        https://www.latex-project.org/
    (    contributing: line  138) ok        https://www.sphinx-doc.org/en/master/
    (python_packaging/pkg_structure: line  702) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
    (python_packaging/pkg_structure: line  787) ok        https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinx.builders.linkcheck.CheckExternalLinksBuilder
    (zzz_archival_docs/hg_version_control: line  208) ok        https://www.selenic.com/mercurial/hgignore.5.html
    (     sphinx_docs: line  260) ok        https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html#module-sphinx.ext.intersphinx
    (zzz_archival_docs/hg_version_control: line  171) ok        https://www.selenic.com/mercurial/hgrc.5.html
    (    contributing: line  138) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
    (python_packaging/pkg_structure: line  702) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports
    (          python: line   20) ok        https://www.tomasbeuzen.com/python-programming-for-data-science/README.html
    (     sphinx_docs: line   57) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html
    (          python: line   20) ok        https://www.tomasbeuzen.com/
    (          xios-2: line  218) ok        https://www.xmlvalidation.com/
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
