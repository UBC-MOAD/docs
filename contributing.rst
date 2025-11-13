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
.. image:: https://app.readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
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
.. _build on readthedocs.org: https://app.readthedocs.org/projects/ubc-moad-docs/builds/


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

.. image:: https://app.readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
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
    Running Sphinx v8.1.3
    loading translations [en]... done
    making output directory... done
    Writing evaluated template result to /media/doug/warehouse/MOAD/docs/_build/html/_static/nbsphinx-code-cells.css
    loading intersphinx inventory 'moadtools' from https://ubc-moad-tools.readthedocs.io/en/latest/objects.inv ...
    loading intersphinx inventory 'nemocmd' from https://nemo-cmd.readthedocs.io/en/latest/objects.inv ...
    loading intersphinx inventory 'salishseacmd' from https://salishseacmd.readthedocs.io/en/latest/objects.inv ...
    loading intersphinx inventory 'salishseadocs' from https://salishsea-meopar-docs.readthedocs.io/en/latest/objects.inv ...
    building [mo]: targets for 0 po files that are out of date
    writing output...
    building [html]: targets for 25 source files that are out of date
    updating environment: [new config] 25 added, 0 changed, 0 removed
    reading sources... [100%] zzz_archival_docs/index
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    copying assets...
    copying static files...
    Writing evaluated template result to /media/doug/warehouse/MOAD/docs/_build/html/_static/language_data.js
    Writing evaluated template result to /media/doug/warehouse/MOAD/docs/_build/html/_static/documentation_options.js
    Writing evaluated template result to /media/doug/warehouse/MOAD/docs/_build/html/_static/basic.css
    Writing evaluated template result to /media/doug/warehouse/MOAD/docs/_build/html/_static/js/versions.js
    copying static files: done
    copying extra files...
    copying extra files: done
    copying assets: done
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

    Running Sphinx v8.1.3
    loading translations [en]... done
    making output directory... done
    loading pickled environment... done
    building [mo]: targets for 0 po files that are out of date
    writing output...
    building [linkcheck]: targets for 25 source files that are out of date
    updating environment: 0 added, 1 changed, 0 removed
    reading sources... [100%] contributing
    looking for now-outdated files... none found
    pickling environment... done
    checking consistency... done
    preparing documents... done
    copying assets...
    copying assets: done
    writing output... [100%] zzz_archival_docs/index

    (          ariane: line   37) -ignored- https://github.com/UBC-MOAD/ariane-2.3.0_03
    (      ssh_access: line   26) -ignored- https://www.baeldung.com/cs/ssh-intro
    (      ssh_access: line  556) -ignored- https://linux.die.net/man/1/scp
    (          xios-2: line   41) -ignored- https://github.com/SalishSeaCast/XIOS-2
    (          xios-2: line  210) -ignored- https://github.com/SalishSeaCast/NEMO-3.6-code
    (          ariane: line   25) ok        http://ariane.lagrangian.free.fr/ariane_tutorial_2.x.x_sep08.pdf
    (          ariane: line   24) ok        http://ariane.lagrangian.free.fr/ariane_namelist_2.x.x_oct08.pdf
    (          ariane: line   23) ok        http://ariane.lagrangian.free.fr/ariane_install_2.x.x_sep08.pdf
    (          ariane: line   15) ok        http://ariane.lagrangian.free.fr/whatsariane.html
    (          ariane: line   37) ok        http://ariane.lagrangian.free.fr/download.php
    (    contributing: line   21) ok        https://app.readthedocs.org/projects/ubc-moad-docs/badge/?version=latest
    (          globus: line   27) ok        https://app.globus.org/file-manager
    (python_packaging/pkg_structure: line  181) ok        https://about.readthedocs.com/?ref=readthedocs.org
    (zzz_archival_docs/hg_version_control: line   41) ok        https://bitbucket.org/
    (git_version_control: line   60) ok        https://brew.sh/
    (    contributing: line   46) ok        https://app.readthedocs.org/projects/ubc-moad-docs/builds/
    (          xios-2: line  405) ok        http://cfconventions.org/Data/cf-standard-names/29/build/cf-standard-name-table.html
    (python_packaging/pkg_structure: line   38) ok        https://blog.ionelmc.ro/2014/05/25/python-packaging/
    (python_packaging/pkg_structure: line   39) ok        https://bskinn.github.io/My-How-Why-Pyproject-Src/
    (alliance-computing: line   55) ok        https://ccdb.alliancecan.ca/account_application
    (alliance-computing: line   85) redirect  https://ccdb.alliancecan.ca/me/access_systems - with Found to https://ccdb.alliancecan.ca/security/login
    (python_packaging/pkg_structure: line  296) ok        https://click.palletsprojects.com/en/latest/
    (github_notebooks_readme: line    7) ok        https://commonmark.org/
    (python_packaging/pkg_structure: line  483) ok        https://calver.org/
    (    contributing: line   13) ok        https://creativecommons.org/licenses/by/4.0/
    (          vscode: line   15) ok        https://code.visualstudio.com/
    (conda_pkg_env_mgr: line   39) ok        https://conda-forge.org/
    (         jupyter: line  356) ok        https://docs.alliancecan.ca/wiki/Anaconda/en
    (python_packaging/pkg_structure: line  164) ok        https://coverage.readthedocs.io/en/latest/
    (alliance-computing: line   69) ok        https://docs.alliancecan.ca/wiki/Getting_started
    (   analysis_repo: line  156) ok        https://cookiecutter.readthedocs.io/en/latest/
    (         jupyter: line  396) ok        https://docs.alliancecan.ca/wiki/Python#Creating_and_using_a_virtual_environment
    (alliance-computing: line   63) ok        https://docs.alliancecan.ca/wiki/Apply_for_a_CCDB_account
    (alliance-computing: line   72) ok        https://docs.alliancecan.ca/wiki/Multifactor_authentication
    (python_packaging/pkg_structure: line  745) ok        https://doc.pytest.org/en/latest/explanation/goodpractices.html#tests-outside-application-code
    (alliance-computing: line  120) ok        https://docs.alliancecan.ca/wiki/SSH_Keys#Using_CCDB
    (conda_pkg_env_mgr: line   15) ok        https://docs.conda.io/en/latest/
    (python_packaging/pkg_structure: line   60) ok        https://docs.conda.io/projects/conda/en/latest/
    (conda_pkg_env_mgr: line  186) ok        https://docs.conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
    (    contributing: line   78) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh
    (      ssh_access: line  506) ok        https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account
    (python_packaging/pkg_structure: line  778) ok        https://docs.github.com/en/code-security/code-scanning/introduction-to-code-scanning/about-code-scanning-with-codeql
    (python_packaging/pkg_structure: line  773) ok        https://docs.github.com/en/actions
    (git_version_control: line   22) ok        https://docs.github.com/en/get-started
    (    contributing: line   13) ok        https://docs.python.org/3.13/
    (python_packaging/pkg_structure: line  761) ok        https://docs.github.com/en/code-security/dependabot
    (oceanparcels/index: line   23) ok        https://docs.oceanparcels.org/en/latest/reference/predefined_kernels.html
    (          xios-2: line  160) ok        https://en.wikipedia.org/wiki/XML
    (python_packaging/pkg_structure: line  376) ok        https://docs.readthedocs.com/platform/stable/config-file/v2.html
    (     bash_config: line   43) ok        https://douglatornell.github.io/2013-09-26-ubc/lessons/ref/shell.html
    (    contributing: line  446) ok        https://git-scm.com/
    (python_packaging/pkg_structure: line  279) ok        https://docs.openstack.org/cliff/latest/
    (git_version_control: line   22) ok        https://git-scm.com/book/en/v2
    (git_version_control: line  151) ok        https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config#ch_core_editor
    (          vscode: line  149) ok        https://fortls.fortran-lang.org/index.html
    (git_version_control: line   22) ok        https://git-scm.com/doc
    (git_version_control: line  213) ok        https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository
    (git_version_control: line   56) ok        https://git-scm.com/downloads
    (git_version_control: line  127) ok        https://git-scm.com/docs/git-config
    ( getting_started: line   55) ok        https://github.com/
    (       vcs_repos: line   19) ok        https://github.com/MIDOSS/
    (oceanparcels/index: line   21) ok        https://github.com/Parcels-code/Parcels/discussions
    (     sphinx_docs: line   36) ok        https://github.com/MIDOSS/docs
    (   analysis_repo: line   62) ok        https://github.com/SalishSeaCast
    (       vcs_repos: line   18) ok        https://github.com/SalishSeaCast/
    (python_packaging/pkg_structure: line   24) ok        https://github.com/SalishSeaCast/NEMO-Cmd
    (          xios-2: line  417) ok        https://github.com/SalishSeaCast/SS-run-sets/tree/main/v201702
    (          xios-2: line  216) ok        https://github.com/SalishSeaCast/SS-run-sets
    (github_notebooks_readme: line    7) ok        https://github.com/SalishSeaCast/analysis-ben/tree/master/notebooks
    (          xios-2: line   41) ok        https://github.com/SalishSeaCast/XIOS-ARCH
    (   analysis_repo: line   33) ok        https://github.com/SalishSeaCast/analysis-susan
    (     sphinx_docs: line   36) ok        https://github.com/SalishSeaCast/docs
    ( getting_started: line   55) ok        https://github.com/UBC-MOAD
    (   analysis_repo: line  316) ok        https://github.com/SalishSeaCast/tools
    (       vcs_repos: line   17) ok        https://github.com/UBC-MOAD/
    (     sphinx_docs: line   79) ok        https://github.com/UBC-MOAD/PythonNotes/blob/main/sphinx-docs/SphinxDocsTutorial.ipynb
    (conda_pkg_env_mgr: line   31) ok        https://github.com/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb
    (python_packaging/pkg_structure: line  296) ok        https://github.com/UBC-MOAD/Reshapr
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs
    (   analysis_repo: line   51) ok        https://github.com/UBC-MOAD/cookiecutter-analysis-repo
    (          xios-2: line  297) ok        https://forge.ipsl.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf
    (          xios-2: line   15) ok        https://forge.ipsl.fr/ioserver/wiki
    (    contributing: line  472) ok        https://github.com/UBC-MOAD/docs/blob/main/CONTRIBUTORS.rst
    (    contributing: line   24) ok        https://github.com/UBC-MOAD/docs/workflows/sphinx-linkcheck/badge.svg
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/issues
    (     sphinx_docs: line   36) ok        https://github.com/UBC-MOAD/moad_tools
    -rate limited-   https://github.com/github/gitignore/blob/main/Python.gitignore | sleeping...
    (conda_pkg_env_mgr: line   39) ok        https://github.com/conda-forge/miniforge
    (python_packaging/pkg_structure: line  419) ok        https://github.com/github/gitignore
    (python_packaging/pkg_structure: line  799) ok        https://github.com/UBC-MOAD/gha-workflows
    (    contributing: line   13) ok        https://github.com/UBC-MOAD/docs/actions?query=workflow:sphinx-linkcheck
    (python_packaging/pkg_structure: line   35) ok        https://hatch.pypa.io/latest/
    (python_packaging/pkg_structure: line  202) ok        https://hatch.pypa.io/latest/config/metadata/
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
    (zzz_archival_docs/hg_version_control: line   27) ok        https://hgbook.red-bean.com/read/how-did-we-get-here.html
    (    contributing: line   15) ok        https://img.shields.io/badge/license-CC--BY-lightgrey.svg
    (    contributing: line   30) ok        https://img.shields.io/badge/python-3.13-blue.svg
    (    contributing: line   18) ok        https://img.shields.io/badge/version%20control-git-blue.svg?logo=github
    (github_notebooks_readme: line   12) ok        https://jupyter.org/
    (         jupyter: line   24) ok        https://jupyter-notebook.readthedocs.io/en/stable/
    (      ssh_access: line   71) ok        https://learn.microsoft.com/en-ca/windows-server/administration/openssh/openssh_install_firstuse
    (          vscode: line  115) ok        https://marketplace.visualstudio.com/items?itemName=Digoro.Clipboard
    (    contributing: line   27) ok        https://img.shields.io/github/issues/UBC-MOAD/docs?logo=github
    (          vscode: line  128) ok        https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv
    (          vscode: line  121) ok        https://marketplace.visualstudio.com/items?itemName=fortran-lang.linter-gfortran
    (          vscode: line  103) ok        https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens
    (python_packaging/pkg_structure: line   37) ok        https://hynek.me/articles/testing-packaging/
    (          vscode: line   88) ok        https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter-renderers
    (          vscode: line   97) ok        https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh
    (          vscode: line  109) ok        https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker
    (          vscode: line  140) ok        https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf
    (          vscode: line  134) ok        https://marketplace.visualstudio.com/items?itemName=trond-snekvik.simple-rst
    (          vscode: line   82) ok        https://marketplace.visualstudio.com/items?itemName=ms-python.python
    (          ariane: line  787) -ignored- https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_TimeRes.ipynb: service unavailable
    (          ariane: line  560) -ignored- https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_Tracers.ipynb: service unavailable
    (          ariane: line  195) -ignored- https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/ParticleTracking.ipynb: service unavailable
    (          xios-2: line   26) ok        https://nemo-cmd.readthedocs.io/en/latest/index.html#nemo-commandprocessor
    (oceanparcels/index: line   24) -ignored- https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/OceanParcelsRecipes.ipynb: service unavailable
    (github_notebooks_readme: line   12) ok        https://nbviewer.org/
    (          xios-2: line  236) ok        https://nemo-cmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (conda_pkg_env_mgr: line   30) -ignored- https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb: service unavailable
    (          python: line   20) ok        https://numpy.org/
    (python_packaging/pkg_structure: line   36) ok        https://packaging.python.org/en/latest/
    (          python: line   20) ok        https://pandas.pydata.org/
    (python_packaging/pkg_structure: line   67) ok        https://packaging.python.org/en/latest/tutorials/installing-packages/#installing-to-the-user-site
    (oceanparcels/index: line   19) ok        https://parcels-code.org/
    (oceanparcels/index: line   13) ok        https://parcels-code.org/index.html
    (python_packaging/pkg_structure: line  483) ok        https://peps.python.org/pep-0440/
    (oceanparcels/index: line   22) ok        https://os.copernicus.org/articles/17/1067/2021/
    (         jupyter: line  413) ok        https://pypi.org/
    (python_packaging/pkg_structure: line   53) ok        https://pip.pypa.io/en/stable/topics/local-project-installs/#editable-installs
    (          python: line   51) ok        https://realpython.com/
    (python_packaging/pkg_structure: line  183) ok        https://pre-commit.com/
    (     sphinx_docs: line  183) ok        https://rhodesmill.org/brandon/2012/one-sentence-per-line/
    (     sphinx_docs: line   78) -ignored- https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/sphinx-docs/SphinxDocsTutorial.ipynb: service unavailable
    (alliance-computing: line  238) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/nibi.html#createworkspaceandclonerepositories
    (alliance-computing: line  242) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/nibi.html#installcommandprocessorpackages
    (alliance-computing: line  248) ok        https://salishsea-meopar-docs.readthedocs.io/en/latest/code-notes/salishsea-nemo/quickstart/nibi.html#compilenemo-3-6-nibi
    (   analysis_repo: line  320) ok        https://salishsea-meopar-tools.readthedocs.io/en/latest/visualisation/visualization_workflows_xarray.html
    (   analysis_repo: line  320) ok        https://salishsea-meopar-tools.readthedocs.io/en/latest/SalishSeaTools/index.html
    (git_version_control: line  151) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FCFR6VU70S
    (oceanparcels/index: line   20) ok        https://salishseacast.slack.com/?redir=%2Farchives%2FC02ETTPHFPX
    (alliance-computing: line  242) ok        https://salishseacmd.readthedocs.io/en/latest/index.html#salishseacmdprocessor
    (     bash_config: line   43) ok        https://software-carpentry.org/
    (          xios-2: line  236) ok        https://salishseacmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
    (python_packaging/pkg_structure: line   40) ok        https://snarky.ca/clarifying-pep-518/
    (    contributing: line   13) ok        https://ubc-moad-docs.readthedocs.io/en/latest/
    (     sphinx_docs: line   50) ok        https://ubc-moad-docs.readthedocs.io/en/latest/sphinx_docs.html#documentation-with-sphinx
    (zzz_archival_docs/hg_version_control: line   75) ok        https://tortoisehg.bitbucket.io/
    (zzz_archival_docs/hg_version_control: line   48) ok        https://support.atlassian.com/bitbucket-cloud/docs/configure-ssh-and-two-step-verification/
    (python_packaging/pkg_structure: line  826) ok        https://tox.wiki/en/latest/
    (zzz_archival_docs/hg_version_control: line  248) ok        https://wiki.mercurial-scm.org/RebaseExtension
    (zzz_archival_docs/hg_version_control: line   27) ok        https://wiki.mercurial-scm.org/BeginnersGuides
    (zzz_archival_docs/hg_version_control: line  248) ok        https://wiki.mercurial-scm.org/RebaseExtension#Scenario_A
    (zzz_archival_docs/hg_version_control: line  262) ok        https://wiki.mercurial-scm.org/RebaseExtension#Scenarios
    (     sphinx_docs: line   88) ok        https://ubc-moad-tools.readthedocs.io/en/latest/pkg_development.html#moad-toolspackageddevelopment
    (conda_pkg_env_mgr: line   15) ok        https://www.anaconda.com/download
    (    CONTRIBUTORS: line    7) ok        https://www.eoas.ubc.ca/~sallen/
    (python_packaging/pkg_structure: line  355) ok        https://www.apache.org/licenses/
    (alliance-computing: line   15) ok        https://www.alliancecan.ca/en
    (          globus: line   15) ok        https://www.globus.org/data-transfer
    (     sphinx_docs: line   19) ok        https://www.mathjax.org/
    (          python: line   15) ok        https://www.python.org/
    (      ssh_access: line   31) ok        https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server
    (zzz_archival_docs/hg_version_control: line   21) ok        https://www.mercurial-scm.org/
    (zzz_archival_docs/hg_version_control: line   63) ok        https://www.mercurial-scm.org/install
    (zzz_archival_docs/hg_version_control: line   75) ok        https://www.sourcetreeapp.com/
    (zzz_archival_docs/hg_version_control: line  208) ok        https://www.selenic.com/mercurial/hgignore.5.html
    (zzz_archival_docs/hg_version_control: line  171) ok        https://www.selenic.com/mercurial/hgrc.5.html
    (     sphinx_docs: line   19) ok        https://www.latex-project.org/
    (    contributing: line  138) ok        https://www.sphinx-doc.org/en/master/
    (python_packaging/pkg_structure: line  702) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html
    (python_packaging/pkg_structure: line  787) ok        https://www.sphinx-doc.org/en/master/usage/builders/index.html#sphinx.builders.linkcheck.CheckExternalLinksBuilder
    (    contributing: line  138) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
    (     sphinx_docs: line   57) ok        https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html
    (python_packaging/pkg_structure: line  702) ok        https://www.sphinx-doc.org/en/master/usage/extensions/autodoc.html#confval-autodoc_mock_imports
    (     sphinx_docs: line  260) ok        https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html#module-sphinx.ext.intersphinx
    (          python: line   20) ok        https://www.tomasbeuzen.com/python-programming-for-data-science/README.html
    (          python: line   20) ok        https://www.tomasbeuzen.com/
    (          xios-2: line  199) ok        https://www.xmlvalidation.com/
    -rate limited-   https://github.com/github/gitignore/blob/main/Python.gitignore | sleeping...
    (         jupyter: line  425) ok        https://github.com/h5netcdf/h5netcdf
    (git_version_control: line  215) ok        https://github.com/github/gitignore/blob/main/Python.gitignore
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
