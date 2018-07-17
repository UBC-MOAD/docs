.. Copyright 2018 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   http://creativecommons.org/licenses/by/4.0/


.. _MOAD-DocsContributing:

**************************
Contributing to These Docs
**************************

Additions,
improvements,
and corrections to these docs are *always* welcome.

The quickest way to fix typos, etc. on existing pages is to use the :guilabel:`Edit on Bitbucket` link in the upper right corner of the page to get to the online editor for the page on `Bitbucket`_.

For more substantial work,
and to add new pages,
the instructions below explain how to:

* clone the repository from `Bitbucket`_

* set up a conda environment in which you can build the docs locally instead of having to push commits to Bitbucket to trigger a `build on readthedocs.org`_

* build the docs with your changes,
  and preview them in Firefox

.. _Bitbucket: https://bitbucket.org/UBC_MOAD/docs
.. _build on readthedocs.org: https://readthedocs.org/projects/ubc-moad-docs/builds/


.. _MOAD-DocsGettingTheRepo:

Getting the Repo
================

Clone the MOAD documentation `repository`_ from Bitbucket with:

.. _repository: https://bitbucket.org/UBC_MOAD/docs

.. code-block:: bash

    $ hg clone ssh://hg@bitbucket.org/UBC_MOAD/docs

or

.. code-block:: bash

    $ hg clone https://you_userid@bitbucket.org/UBC_MOAD/docs

if you don't have `ssh key authentication`_ set up on Bitbucket
(replace :kbd:`you_userid` with you Bitbucket userid,
or copy the link from the :guilabel:`Clone` action pop-up on the `repository`_ page).

.. _ssh key authentication: https://confluence.atlassian.com/bitbucket/set-up-ssh-for-mercurial-728138122.html


.. _MOAD-DocsBuildEnvironment:

Docs Build Environment
======================

Setting up an isolated docs build environment using `Conda`_ is recommended.
Assuming that you have the `Anaconda Python Distribution`_ or `Miniconda3`_ installed,
you can create and activate an environment called :kbd:`moad-docs` that will have all of the Python packages necessary for building the documentation with the commands:

.. _Conda: http://conda.pydata.org/docs/
.. _Anaconda Python Distribution: https://www.continuum.io/downloads
.. _Miniconda3: http://conda.pydata.org/docs/install/quick.html

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

The MOAD documentation is written in `reStructuredText`_ and converted to HTML using `Sphinx`_.
Creating a :ref:`MOAD-DocsBuildEnvironment` as described above includes the installation of Sphinx.
Building the documentation is driven by the :file:`docs/Makefile`.
With your :kbd:`moad-docs` environment activated,
use:

.. _reStructuredText: http://sphinx-doc.org/rest.html
.. _Sphinx: http://sphinx-doc.org/

.. code-block:: bash

    (moad-docs)$ make clean html

to do a clean build of the documentation.
The output looks something like::

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

If you have write access to the `repository`_ on Bitbucket,
whenever you push changes to Bitbucket the documentation is automatically re-built and rendered at http://ubc-moad-docs.readthedocs.io/en/latest/.
