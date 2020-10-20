.. Copyright 2018-2020 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MOAD-Jupyter:

*******
Jupyter
*******

`Project Jupyter`_ is a collection of open-source software,
open-standards,
and services for interactive computing across a variety of programming languages.
We use Jupyter with Python as a core tool for analysis of model results and ocean observations,
and communication of those analyses within the MOAD group,
and with collaborators.

.. _Project Jupyter: https://jupyter.org/

`Jupyter Notebooks`_ are the core feature of Jupyter.
Notebooks contain live Python code,
equations,
visualizations,
and narrative text.
They are an excellent tool for data exploration and analysis,
teaching,
communication,
and early-stage code development.

.. _Jupyter Notebooks: https://jupyter-notebook.readthedocs.io/en/stable/

Jupyter is a server-client system,
even when you are running it on your own laptop.
The server part runs in a terminal window and is called the kernel.
It's the part that runs Python.
The client part runs in your web browser.
It's the part where you type in code,
narrative text,
LaTeX equations,
etc.
and where you see the code results and visualization images.

Jupyter provides more than one user interface,
among them:

* The original :command:`jupyter notebook` interface opens a file navigator in one tab of your browser.
  Clicking on a notebooks in that navigator tab causes it to open in another browser tab.
  You can open as many notebook tabs as you want -
  at least until your computer runs out of memory!

* The :command:`jupyter lab` is the newer "next generation" interface that puts everything in one browser tab with multiple panes that you can move around and re-size.
  In addition to notebooks,
  :kbd:`lab` also provides a text editor,
  code consoles that are synced with notebooks,
  and terminal panes that give access to your system shell,
  just like your terminal program does.

Unless you have a strong reason to use the original :kbd:`notebook` interface,
you should use the :kbd:`lab` interface because that is the part of Jupyter that is being most actively developed,
and where new features are most likely to appear.

Notebooks aren't the only way to use Python though!
Code that is frequently used,
that is hundreds of lines long,
or that takes a significant amount of time to run
is often best moved from notebooks into Python modules and packages.
Doing so makes the code more maintainable,
testable,
and more efficient to run,
but at the expense of separating the code from equations,
visualizations,
and narrative text.
Those important things have to find a home in documentation that accompanies the code modules.


.. _RunningJupyterLocally:

Running Jupyter Locally
=======================

Jupyter is included in the `Anaconda Python Distribution`_.
If are using `Miniconda`_ to create and manage your Python environments,
you can install Jupyter by adding the :kbd:`jupyterlab` package to your environment description YAML file.

.. _Anaconda Python Distribution: https://www.anaconda.com/products/individual
.. _Miniconda: https://docs.conda.io/en/latest/miniconda.html

In a terminal window,
go to the directory that you want to be at the top level of Jupyter's file navigation,
and start the Jupyter server.
For example,
if you are working in your analysis repo,
the commands would be like:

.. code-block:: bash

    $ cd analysis-doug/
    $ jupyter lab

The terminal window that you typed those commands into is now running the server part of Jupyter.
You have to keep it open until you are finished with Jupyter and want to shut it down.

The client part of Jupyter should have opened in a new browser tab.
If not,
follow the instructions in the terminal window that say something like::

      To access the notebook, open this file in a browser:
        file:///home/doug/.local/share/jupyter/runtime/nbserver-3581193-open.html
    Or copy and paste one of these URLs:
        http://localhost:8889/?token=f8b14419fc17ff93240a914930566fad4c2f69f064d4fdb9
     or http://127.0.0.1:8889/?token=f8b14419fc17ff93240a914930566fad4c2f69f064d4fdb9

For the older :kbd:`notebook` interface,
the instructions are much the same,
but the command to start the server is :command:`jupyter notebook`.

When you are finished using Jupyter,
save your notebook(s) via the menu in the browser tab,
close the tab,
and use :kbd:`Ctrl-C` in the terminal window to shut down the Jupyter server.

Don't forget to commit your work in :program:`git` and push your changes to GitHub!


.. _RunningJupyterRemotely-MOAD:

Running Jupyter Remotely on :kbd:`salish` or a MOAD Workstation
===============================================================

**TODO**


.. _RunningJupyterRemotely-ComputeCanada:

Running Jupyter Remotely on :kbd:`graham`
=========================================

**TODO**
