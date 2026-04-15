.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


Why Pixi?
=========

If you have worked with Python before,
you might have heard of tools like ``pip``, ``conda``, or ``venv``.
Pixi provides several advantages over these traditional tools:

* **Reproducibility**:
  Pixi uses a "lock file" (:file:`pixi.lock`) to ensure that the exact same versions of all packages
  can be reliably installed by different users on different machines.
  This prevents "it works on my machine" bugs.
* **Speed**:
  Pixi is built in Rust and is exceptionally fast at resolving and installing packages.
* **Multi-language**:
  While excellent for Python, Pixi can also manage packages from other languages and systems (like C++, R, or command-line tools).
* **Isolated Environments**:
  Pixi creates a separate environment for each project,
  so you don't have to worry about one project's requirements breaking another.


.. _InstallingPixi:

Installing Pixi
===============

The easiest way to install Pixi is using their official installer script.


On macOS and Linux:
-------------------

.. code-block:: console

    $ curl -fsSL https://pixi.sh/install.sh | bash


On Windows (PowerShell):
------------------------

.. code-block:: powershell

    iwr -useb https://pixi.sh/install.ps1 | iex


Autocompletion
--------------

Having Pixi autocomplete its sub-commands,
option flags,
environment names,
etc.
when you hit :kbd:`Tab` is very convenient.
To enable that for :program:`bash`,
add the following line to your :file:`$HOME/.bashrc` file:

.. code-block:: bash

    eval "$(pixi completion --shell bash)"

For other shells,
please see the `Pixi Autocompletion`_ docs.

.. _Pixi Autocompletion: https://pixi.prefix.dev/latest/installation/#autocompletion

After installation, please restart your terminal session to finalize the installation.


Basic Usage
===========

Once Pixi is installed, here are the most common commands you will use.


Installing an Environment
-------------------------

After you have cloned a Pixi-powered repository, navigate to your project folder and run:

.. code-block:: console

    $ pixi install

This downloads the packages required by the project and installs them in an isolated environment
for you to use.


Adding Packages
---------------

To add a package (like ``numpy`` or ``pandas``) to your project:

.. code-block:: console

    $ pixi add numpy

This will update your :file:`pixi.toml` and :file:`pixi.lock` files,
and automatically install the package into the project-specific environment.
Please ensure that you commit the changes your :file:`pixi.toml` and :file:`pixi.lock` files,
and push them to GitHub so that you maintain an accurate record of you working environment.


Running Commands
----------------

To run a script or a command within your Pixi environment:

.. code-block:: console

    $ pixi run python my_script.py


Using the Pixi Shell
--------------------

If you are doing a lot of command-line work and don't want to have to type :command:`pixi run`
before every command,
you can work inside the environment interactively by doing:

.. code-block:: console

    $ pixi shell

This "activates" the environment in your current terminal session.
You can exit the shell by typing ``exit``.


Pixi Details
============

Pixi has many more features and capabilities.
Most MOAD users will use Pixi by cloning Pixi-powered repositories from GitHub and
running commands with :command:`pixi run`,
but Pixi also has extensive support for Python package development and other software
development tasks.
The `documentation`_ has excellent "Getting Started" sections and tutorials,
as well as detailed `command-line interface (CLI) reference docs`_.

.. _documentation: https://pixi.prefix.dev/latest/
.. _command-line interface (CLI) reference docs: https://pixi.prefix.dev/latest/reference/cli/pixi/
