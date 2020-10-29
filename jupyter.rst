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


.. _RunningJupyterRemotely:

Running Jupyter Remotely
========================

:ref:`RunningJupyterLocally` is fine if your laptop has enough compute power for the code you are trying to run,
and if you have the data or model results files you want to work on stored on your drive.
However,
it is often better to "take the compute to the data" rather than download large data files to your laptop,
and rely on its CPU cores for calculations.
Remote machines like the MOAD workstations,
our development server :kbd:`salish`,
and the compute nodes on the Compute Canada :kbd:`graham` cluster
have more and faster CPU cores than most laptops,
and access to far larger storage.

Fortunately,
the server-client structure of Jupyter makes it relatively easy to use the CPU cores and storage of a remote system with the user interface in the browser on your laptop.
We do that by running the server part on a remote system,
and using :command:`ssh` to create a secure access "tunnel" between the server and our laptop to allow the client part running in our local browser to connect to the remote server part.

Apart from access to more compute power and avoiding the need to move large files around,
using :command:`jupyter lab` on a remote system has other advantages:

* You can open terminal panes in the :kbd:`lab` interface to give you a terminal session on the remote machine for things like file system tasks: copying or moving files, managing permissions, etc.,
  for :command:`git` version control tasks: pulls, commits, and pushes,
  or anything else you need to do in a command-line interface.

* You can open editor panes in the :kbd:`lab` interface to work on files stored on the remote system.
  Doing that avoids the need to copy files back and forth between your laptop and the remote system,
  or deal with network lag when you try to use a full-screen editor in a remote desktop session.
  You can use the :guilabel:`Settings > Text Editor Key Map` menu in :kbd:`lab` to set the editor keyboard mapping to your choice of :program:`vim`,
  :program:`emacs`,
  or :program:`Sublime Text`.


.. _RunningJupyterRemotely-MOAD:

Running Jupyter Remotely on :kbd:`salish` or a MOAD Workstation
---------------------------------------------------------------

This section assumes that you have installed the `Anaconda Python Distribution`_
in your :envvar:`$HOME` directory on a MOAD workstation,
or that you are working in :program:`conda` environment that includes the :kbd:`jupyterlab` package on a MOAD workstation.

.. note::
    You don't need to install Anaconda Python or :kbd:`jupyterlab` explicitly on :kbd:`salish` if you have already installed it on a MOAD workstation because :kbd:`salish` uses the same :envvar:`$HOME` file system as all of the MOAD workstations.

It is also assumed that you have followed the instructions in the :ref:`SetUpSshConfiguration` section to set up host aliases for :kbd:`salish` and any other workstations you want to run the :command:`jupyter lab` server on.

You can use the technique in this section to run the :command:`jupyter lab` server on any of the MOAD workstations by replacing :kbd:`salish` with the workstation name.
:kbd:`salish` has the advantages of having lots of compute power
(16 3.2 MHz cores running 2 threads each,
and 256 Gb of memory)
and of being close physically and in network terms to our large storage arrays :file:`/data/`,
:file:`/results/`,
:file:`/results2/`,
:file:`/opp/`,
and :file:`/ocean/`.
That said,
the MOAD workstations have ample compute power and are nearly as fast access to the storage arrays,
so they are well up to the task of running the :command:`jupyter lab` for analysis work.

To start the :command:`jupyter lab` server on :kbd:`salish`,
open a terminal window on your laptop,
and use :program:`ssh` to start a command-line session on :kbd:`salish`:

.. code-block:: bash

    $ ssh salish

Once you are connected to :kbd:`salish`,
navigate to the directory that you want to be at the top level of Jupyter's file navigation,
and start the Jupyter server.
For example,
if you are working in your analysis repo,
the commands would be like:

.. code-block:: bash

    $ cd analysis-doug/
    $ jupyter lab --no-browser

The :kbd:`--no-browser` option in that command tells :program:`jupyter` to start the server part only,
and not to start the client part in a browser.

You should see output in that terminal window that looks something like:

.. code-block:: text

    [I 09:30:01.331 LabApp] JupyterLab extension loaded from /home/dlatorne/conda_envs/dask-expts/lib/python3.8/site-packages/jupyterlab
    [I 09:30:01.332 LabApp] JupyterLab application directory is /home/dlatorne/conda_envs/dask-expts/share/jupyter/lab
    [I 09:30:01.362 LabApp] Serving notebooks from local directory: /data/dlatorne/analysis-doug/
    [I 09:30:01.362 LabApp] Jupyter Notebook 6.1.4 is running at:
    [I 09:30:01.363 LabApp] http://localhost:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
    [I 09:30:01.363 LabApp]  or http://127.0.0.1:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
    [I 09:30:01.363 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 09:30:01.381 LabApp]

        To access the notebook, open this file in a browser:
            file:///home/dlatorne/.local/share/jupyter/runtime/nbserver-1998772-open.html
        Or copy and paste one of these URLs:
            http://localhost:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
         or http://127.0.0.1:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1

.. note::
    Keep this terminal window open.
    It is where the Jupyter server part is running.
    If you close it,
    you will shutdown the Jupyter server and your :command:`jupyter lab` session will stop working.

The URLs on the last 2 lines are the important bit that we need to use to get the client running on our laptop.
Those 2 URLs are just two different ways of spelling the same thing.
:kbd:`localhost` is a synonym for the IP address :kbd:`127.0.0.1`.
They both mean "the computer where this process is running"; i.e. :kbd:`salish` in this case.

The number after the colon in the URLs
(:kbd:`8888` above)
is the port number that the Jupyter server is running on.
:kbd:`8888` is the default,
but if that port is busy,
probably because somebody else is already running a Jupyter server on it,
Jupyter will choose a different port number.
You need to use the port number that *your* Jupyter server server is running on in the next step when we set up the :program:`ssh` tunnel between your laptop and :kbd:`salish` for the Jupyter client to use.

To set up the :program:`ssh` tunnel,
open a new terminal window on your laptop,
and enter the command:

.. code-block:: bash

    ssh -N -L 4343:localhost:8888 salish

This use of :program:`ssh` is called "port forwarding", or "ssh tunnelling".
It creates an ssh encrypted connection between a port on your laptop
(port :kbd:`4343` on :kbd:`localhost` in this case)
and a port on the remote host
(port :kbd:`8888` on :kbd:`salish` in this case).
The :kbd:`-N` option tells :program:`ssh` not to execute a command on the remote system because all we want to do is set up the port forwarding.
The :kbd:`-L` option tells :program:`ssh` that the next blob of text is the details of the port forwarding to set up.

You can use any number :kbd:`≥1024` you want instead of :kbd:`4343` as the local port number on your laptop.
The number after :kbd:`:localhost:` has to be the same as the port number in the message that the Jupyter server printed out.

.. note::
    Keep this terminal window open too.
    If you close it,
    you will collapse the :program:`ssh` port forwarding tunnel and your Jupyter server and client will stop being able to talk to each other.

Finally,
open a new tab in the browser on your laptop and go to http:://localhost:4343/ to bring up the Jupyter client.
Use whatever port number you chose,
if you chose to use something other than :kbd:`4343` in the :command:`ssh -N -L ...` command.

When you run Jupyter in this way,
remember that all of the notebooks and files you are working with are on the remote computer (:kbd:`salish`) file system,
not on your laptop.
So,
when you commit your changes with :program:`git`,
do it in a terminal session on the remote machine
(either inside Jupyter,
or in a new :program:`ssh` session).

When you are finished using Jupyter:

#. save your notebooks
#. close the browser tab
#. go to the terminal window on the remote machine where the Jupyter server is running,
   and hit :kbd:`Ctrl-c` to stop the Jupyter server
#. go to the terminal window on your laptop where you ran :command:`ssh -N -L ...`,
   and hit :kbd:`Ctrl-c` to end the port forwarding


.. _RunningJupyterRemotely-ComputeCanada:

Running Jupyter Remotely on :kbd:`graham`
-----------------------------------------

**TODO**
