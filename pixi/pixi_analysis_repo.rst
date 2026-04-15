.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MigratingYourAnalysisRepositoryToPixi:

******************************************
Migrating Your Analysis Repository to Pixi
******************************************

The steps top migrate your analysis repository to Pixi are:

#. Initialize your repository as a Pixi workspace
#. Test a notebook
#. Clean up the default environment
#. Commit and push :file:`pixi.toml`, :file:`pixi.lock`, and removal of :file:`notebooks/environment.yaml`
#. Import any other environment you may have
#. Remove conda if you have removed all your environments

The steps above assume that you have already :ref:`Installed Pixi<InstallingPixi>`


Initialize Your Repository as a Pixi Workspace
==============================================

These command assume that you are working on ``salish`` or a Waterhole workstation.
They use :file:`analysis-casey/` as the analysis repository.
Yours,
of course,
will use your name.

.. code-block:: console

    $ cd /ocean/$USER/MOAD/analysis-casey/
    $ pixi init --import notebooks/environment.yaml
    $ pixi add \
        --git https://github.com/SalishSeaCast/tools.git \
        --pypi \
        --subdir SalishSeaTools \
        SalishSeaTools

.. note::
   Depending on when you set up your analysis repository,
   it is possible that its path is different to :file:`/ocean/$USER/MOAD/analysis-<your-name>/`.


Test a Notebook
===============

If you run Jupyter from the commandline:

.. code-block:: console

    $ pixi run jupyter lab notebooks/my_notebook.ipynb

If you us VS Code:

* Install the `Pixi Code extension`_

  .. _Pixi Code extension: https://marketplace.visualstudio.com/items?itemName=renan-r-santos.pixi-code

* The :guilabel:`Select Kernel` button in VS Code should show the kernel in your workspace as a choice,
  probably :file:`.pixi/envs/default/bin/python`


Clean Up the Default Environment
================================

You no longer need the :file:`notebooks/environment.yaml` file,
note the ``analysis-casey`` conda environment.
So,
remove them:

.. code-block:: console

    $ rm notebooks/environment.yaml
    $ conda env remove -n analysis-casey


Commit and Push
===============

The steps above will have created 3 new files:

* :file:`pixi.toml` - the Pixi manifest
* :file:`pixi.lock` - the Pixi lockfile
* :file:`.gitattributes` - settings to control how Git manages :file:`pixi.lock`

removed 1 file:

* :file:`notebooks/environment.yaml` - conda environment description file

and modified 1 file:

* :file:`.gitignore`

Stage and commit those files in the VS Code Commit side-panel or at the commandline,
and push them to GitHub:

.. code-block:: console

    $ git add pixi.toml pixi.lock .gitattributes .gitignore notebooks/environment.yaml
    $ git commit -m "Change to Pixi for dependency and environment management"
    $ git push


Import Any Other Environments
=============================

Please see the :ref:`ImportingCondaEnvironmentIntoPixiWorkspace` section.


Maybe Remove Conda
==================

If the output of:

.. code-block:: console

   $ conda env list

shows only the ``base`` environment like:

.. code-block:: output
  :class: no-copybutton

   # conda environments:
   #
   base                     /home/allen/miniforge3

follow the `Uninstall instructions`_ for Miniforge to remove it.

.. _Uninstall instructions: https://github.com/conda-forge/miniforge#uninstall

If you have environments in addition to ``base`` left,
migrate them to Pixi before you remove conda.
Please contact Doug if you need help.
