.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _AddingNewPixiEnvironment:

*****************************
Adding a New Pixi Environment
*****************************

As an example of adding a new Pixi environment to and existing workspace,
we'll add an environment for running `Parcels`_

.. _Parcels: https://docs.parcels-code.org/en/latest/installation.html#basic-installation

The installation instructions that `Parcels`_ provides include the instruction to create a
Conda environment with:

.. code-block:: console
   :class: no-copybutton

    $ conda create -n parcels -c conda-forge parcels trajan cartopy jupyter

The equivalent Pixi commands are:

.. code-block:: console

    $ pixi add --feature parcels parcels trajan cartopy jupyter
    $ pixi workspace environment add parcels --feature parcels --no-default-feature
    $ pixi install -e parcels

The first command adds a `Pixi Feature`_ named ``parcels`` that has 4 packages,
``parcels``,
``trajan``,
``cartopy``,
and ``jupyter`` as dependencies.
This command updates the :file:`pixi.toml` manifest file.

.. _Pixi Feature: https://pixi.prefix.dev/latest/tutorials/multi_environment/#feature

The second command adds a `Pixi Environment`_ named ``parcels`` that includes the ``parcels`` feature.
The ``--no-default-feature`` option isolates the ``parcels`` environment from other environments in the
workspace.
This command also updates the :file:`pixi.toml` manifest file.

.. _Pixi Environment: https://pixi.prefix.dev/latest/tutorials/multi_environment/#environment

The third command downloads the dependency packages
(if necessary)
and installs them in the ``parcels`` environment.
This command updates the :file:`pixi.lock` lock file.

Commit and push the changes in the :file:`pixi.toml` and :file:`pixi.lock` files.

To run a command in the ``parcels`` environment,
use the ``-e parcels`` or ``--environment parcels`` option in the :command:`pixi run` command.
Example:

.. code-block:: console

    $ pixi run -e parcels python example_peninsula.py --fieldset 100 100
