.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _ImportingCondaEnvironmentIntoPixiWorkspace:

***************************************************
Importing a Conda Environment Into a Pixi Workspace
***************************************************

If the :file:`erddap-obs-matching/environment.yaml` file describes an environment called ``erddap-obs-matching``,
you can import that environment into your workspace with:

.. code-block:: console

    $ pixi import erddap-obs-matching/environment.yaml

That command:
  * adds a Pixi feature called ``erddap-obs-matching``
  * adds channels and dependencies from the :file:`erddap-obs-matching/environment.yaml` file to the feature
  * adds the feature to a Pixi environment called ``erddap-obs-matching``

Then,

.. code-block:: console

  $ pixi install -e erddap-obs-matching

creates the ``erddap-obs-matching`` environment and installs the dependencies,
updating the Pixi lock file with those details to ensure reproducibility.

You can run commands in the environment with commands like:

.. code-block:: console

    $ pixi run -e erddap-obs-matching python -m my_module

When you are happy that the environment is working as you expect,
clean up by removing the :file:`erddap-obs-matching/environment.yaml` environment description file,
and the :file:`erddap-obs-matching/environment.yaml` conda environment:

.. code-block:: console

    $ rm erddap-obs-matching/environment.yaml
    $ conda env remove -n erddap-obs-matching

Then commit and push the changes to :file:`pixi.toml` and :file:`pixi.lock`,
and the removal of :file:`erddap-obs-matching/environment.yaml`.
