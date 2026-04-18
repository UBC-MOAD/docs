.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MigratingToPixiPoweredSalishSeaCmd:

**************************************
Migrating to Pixi-Powered SalishSeaCmd
**************************************

If you are running NEMO on one of the Alliance HPC clusters,
an easy way to start using Pixi is to migrate your :py:obj:`SalishSeaCmd` installation
and workflow to Pixi.
The steps are:

#. Install Pixi on the Alliance cluster
   (if you have not already done so)
#. Pull the most recent version of :py:obj:`SalishSeaCmd` from GitHub
#. Use Pixi to to create an isolated environment for :py:obj:`SalishSeaCmd`
#. Use :command:`pixi run salishsea run...` to launch a run
#. Clean up the elements of your old conda-powered installation


Install Pixi
============

.. code-block:: console

    $ curl -fsSL https://pixi.sh/install.sh | bash

Having Pixi autocomplete its sub-commands,
option flags,
environment names,
etc.
when you hit :kbd:`Tab` is very convenient.
To enable that,
add the following line to your :file:`$HOME/.bashrc` file:

.. code-block:: bash

    eval "$(pixi completion --shell bash)"

After installation, please restart your terminal session to finalize the installation.


Update :py:obj:`SalishSeaCmd`
=============================

.. code-block:: console

    $ cd $HOME/SalishSeaCmd/
    $ git pull

.. note::
   Depending on when you set up :py:obj:`SalishSeaCmd` on the cluster,
   it is possible that its path is different to :file:`$HOME/MEOPAR/SalishSeaCmd/`.
   The next most likely possibility is :file:`$PROJECT/$USER/MEOPAR/SalishSeaCmd/`.


Create the :py:obj:`SalishSeaCmd` Environment
=============================================

.. code-block:: console

    $ pixi install


Use :command:`pixi run salishsea run...`
========================================

When you are in the :file:`SalishSeaCmd/` directory
(or a sub-directory)
you can run the :program:`salishsea` command with with the :command:`pixi run` command.
Example:

.. code-block:: console

    $ pixi run salishsea help

A common use-case is to execute the :command:`salishsea run` command in the directory containing
your run description YAML file.
To accomplish that,
you have to tell Pixi where to find the :file:`SalishSeaCmd/` directory so that it can use the
correct environment.
You do that by using the ``-m`` or ``--manifest`` option of :command:`pixi run`.
Example:

.. code-block:: console

    $ cd SS-run-sets/SalishSea/sea/Carbon_v202111/
    $ pixi run -m $HOME/MEOPAR/SalishSeaCmd salishsea run 01jan11_Lb80.yaml \
        /scratch/allen/Carbon/MoreSens/Now/01jan11/


Cleanup
=======

#. Uninstall :py:obj:`SalishSeaCmd` from :file:`$HOME/.local/bin/`

   .. code-block:: console

      $ conda activate salishsea-cmd
      $ python -m pip uninstall SalishSeaCmd

#. Uninstall :py:obj:`NEMO-Cmd` from :file:`$HOME/.local/bin`

   .. code-block:: console

      $ python -m pip uninstall NEMO-Cmd

#. Remove your ``salishsea-cmd`` conda environment

   .. code-block:: console

      $ conda deactivate
      $ conda env remove -n salishsea-cmd

#. Remove your :py:obj:`NEMO-Cmd` repository clone

   Installation and updating of :py:obj:`NEMO-Cmd` is now handled by Pixi because :py:obj:`NEMO-Cmd`
   is now an implicit dependency for :py:obj:`SalishSeaCmd` in its manifest file.
   So,
   you can remove your :py:obj:`NEMO-Cmd` repository clone:

   .. code-block:: console

      $ rm -rf $HOME/MEOPAR/NEMO-Cmd/

   .. note::
      Depending on when you set up :py:obj:`SalishSeaCmd` on the cluster,
      it is possible that the path is different to :file:`$HOME/MEOPAR/NEMO-Cmd/`.
      The next most likely possibility is :file:`$PROJECT/$USER/MEOPAR/NEMO-Cmd/`.

#. Remove ``NEMO-Cmd`` from the ``vcs revisions:`` sections of your run description YAML files.

   If you don't you will get warnings like:

   .. code-block:: output
      :class: no-copybutton

      nemo_cmd.prepare WARNING: revision and status requested for non-existent repo: /home/dlatorne/MEOPAR/NEMO-Cmd

   every time you use :command:`pixi run salishsea run ...`

#. Remove conda (if the only environment left is ``base``)

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
