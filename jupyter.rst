.. Copyright 2018 – present by The UBC EOAS MOAD Group
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
  ``lab`` also provides a text editor,
  code consoles that are synced with notebooks,
  and terminal panes that give access to your system shell,
  just like your terminal program does.

Unless you have a strong reason to use the original ``notebook`` interface,
you should use the ``lab`` interface because that is the part of Jupyter that is being most actively developed,
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

Assuming that you have :ref:`Installed Miniforge <InstallingMiniforge>` so that
you are using :command:`conda`  to create and manage your Python environments,
you can install Jupyter by adding the ``jupyterlab`` package to your environment description YAML file.
For example,
the :file:`notebooks/environment.yaml` file in your analysis repo includes ``jupyterlab``.

In a terminal window,
go to the directory that you want to be at the top level of Jupyter's file navigation,
and start the Jupyter server.
For example,
if you are working in your analysis repo,
the commands would be like:

.. code-block:: bash

    $ conda activate analysis-doug
    (analysis-doug)$ cd analysis-doug/
    (analysis-doug)$ jupyter lab

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

For the older ``notebook`` interface,
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
our development server ``salish``,
and the compute nodes on the Compute Canada ``graham`` cluster
have more and faster CPU cores than most laptops,
and access to far larger storage.

Fortunately,
the server-client structure of Jupyter makes it relatively easy to use the CPU cores and storage of a remote system with the user interface in the browser on your laptop.
We do that by running the server part on a remote system,
and using :command:`ssh` to create a secure access "tunnel" between the server and our laptop to allow the client part running in our local browser to connect to the remote server part.

Apart from access to more compute power and avoiding the need to move large files around,
using :command:`jupyter lab` on a remote system has other advantages:

* You can open terminal panes in the ``lab`` interface to give you a terminal session on the remote machine for things like file system tasks: copying or moving files, managing permissions, etc.,
  for :command:`git` version control tasks: pulls, commits, and pushes,
  or anything else you need to do in a command-line interface.

* You can open editor panes in the ``lab`` interface to work on files stored on the remote system.
  Doing that avoids the need to copy files back and forth between your laptop and the remote system,
  or deal with network lag when you try to use a full-screen editor in a remote desktop session.
  You can use the :guilabel:`Settings > Text Editor Key Map` menu in ``lab`` to set the editor keyboard mapping to your choice of :program:`vim`,
  :program:`emacs`,
  or :program:`Sublime Text`.


.. _RunningJupyterRemotely-MOAD:

Running Jupyter Remotely on ``salish`` or a MOAD Workstation
------------------------------------------------------------

This section assumes that you have :ref:`Installed Miniforge <InstallingMiniforge>`
in your :envvar:`$HOME` directory on a MOAD workstation,
or that you are working in :program:`conda` environment that includes the ``jupyterlab`` package on a MOAD workstation.

.. note::
    You don't need to :ref:`Install Miniforge <InstallingMiniforge>` or ``jupyterlab``
    explicitly on ``salish`` if you have already installed it on a MOAD workstation because ``salish`` uses the same :envvar:`$HOME` file system as all of the MOAD workstations.

It is also assumed that you have followed the instructions in the :ref:`SetUpSshConfiguration` section to set up host aliases for ``salish`` and any other workstations you want to run the :command:`jupyter lab` server on.

You can use the technique in this section to run the :command:`jupyter lab` server on any of the MOAD workstations by replacing ``salish`` with the workstation name.
``salish`` has the advantages of having lots of compute power
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

To start the :command:`jupyter lab` server on ``salish``,
open a terminal window on your laptop,
and use :program:`ssh` to start a command-line session on ``salish``:

.. code-block:: bash

    $ ssh salish

Once you are connected to ``salish``,
navigate to the directory that you want to be at the top level of Jupyter's file navigation,
and start the Jupyter server.
For example,
if you are working in your analysis repo,
the commands would be like:

.. code-block:: bash

    $ cd analysis-doug/
    $ jupyter lab --no-browser --ip $(hostname -f)

The ``--no-browser`` option in that command tells :program:`jupyter` to start the server part only,
and not to start the client part in a browser.
The ``--ip $(hostname -f)`` causes the name of the machine you are running the server on to be used in the URLs that Jupyter sets up for the server.

You should see output in that terminal window that looks something like:

.. code-block:: text

    [I 09:30:01.331 LabApp] JupyterLab extension loaded from /home/dlatorne/conda_envs/dask-expts/lib/python3.8/site-packages/jupyterlab
    [I 09:30:01.332 LabApp] JupyterLab application directory is /home/dlatorne/conda_envs/dask-expts/share/jupyter/lab
    [I 09:30:01.362 LabApp] Serving notebooks from local directory: /data/dlatorne/analysis-doug/
    [I 09:30:01.362 LabApp] Jupyter Notebook 6.1.4 is running at:
    [I 09:30:01.363 LabApp] http://salish:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
    [I 09:30:01.363 LabApp]  or http://127.0.0.1:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
    [I 09:30:01.363 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 09:30:01.381 LabApp]

        To access the notebook, open this file in a browser:
            file:///home/dlatorne/.local/share/jupyter/runtime/nbserver-1998772-open.html
        Or copy and paste one of these URLs:
            http://salish:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1
         or http://127.0.0.1:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1

.. note::
    Keep this terminal window open.
    It is where the Jupyter server part is running.
    If you close it,
    you will shutdown the Jupyter server and your :command:`jupyter lab` session will stop working.

The URLs on the last 2 lines are the important bit that we need to use to get the client running on our laptop.
The second last one that contains the name of the machine that the server is running on is the important one for the rest of this setup.
That is::

  http://salish:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1

in the example output above.

The number after ``salish:`` in the URL
(``8888`` above)
is the port number that the Jupyter server is running on.
``8888`` is the default,
but if that port is busy,
probably because somebody else is already running a Jupyter server on it,
Jupyter will choose a different port number.
You need to use the port number that *your* Jupyter server server is running on in the next step when we set up the :program:`ssh` tunnel between your laptop and ``salish`` for the Jupyter client to use.

To set up the :program:`ssh` tunnel,
open a new terminal window on your laptop,
and enter the command:

.. code-block:: bash

    $ ssh -N -L 4343:salish:8888 salish

This use of :program:`ssh` is called "port forwarding", or "ssh tunnelling".
It creates an ssh encrypted connection between a port on your laptop
(port ``4343`` in this case)
and a port on the remote host
(port ``8888`` on ``salish`` in this case).
The ``-N`` option tells :program:`ssh` not to execute a command on the remote system because all we want to do is set up the port forwarding.
The ``-L`` option tells :program:`ssh` that the next blob of text is the details of the port forwarding to set up.

You can use any number ``≥1024`` you want instead of ``4343`` as the local port number on your laptop.
The number after ``:salish:`` has to be the same as the port number in the URLs that the Jupyter server printed out.

.. note::
    Keep this terminal window open too.
    If you close it,
    you will collapse the :program:`ssh` port forwarding tunnel and your Jupyter server and client will stop being able to talk to each other.

.. note::
    Remember that if you are running the server part of Jupyter on a MOAD workstation like ``char`` rather than on ``salish``,
    you need to use the workstation name in 2 places in the :command:`ssh -N -L ...` command.

Finally,
open a new tab in the browser on your laptop and go to ``http://localhost:4343/`` to bring up the Jupyter client.
Use whatever port number you chose,
if you chose to use something other than ``4343`` in the :command:`ssh -N -L ...` command.
You may land on a Jupyter page that asks you to enter a :guilabel:`Password or token` to log in.
If so,
copy the the long string of digits and letters from the URL in the Jupyter server terminal windows.
For example,
the in the URL::

  http://sailsh:8888/?token=bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1

the token is ``bbd686ffaa5398aacaee25c9fa44b5f9424889a81ad7d9f1``.

When you run Jupyter in this way,
remember that all of the notebooks and files you are working with are on the remote computer (``salish``) file system,
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

Running Jupyter Remotely on ``graham``
-----------------------------------------

This section assumes that you have followed the instructions in the :ref:`SetUpSshConfiguration` section to set up host aliases for ``graham`` and any other Compute Canada clusters you want to run the :command:`jupyter lab` server on.

You can use the technique in this section to run the :command:`jupyter lab` server on any of the Compute Canada clusters by replacing ``graham`` with the cluster name.

The recommended way to run a :command:`jupyter lab` server on ``graham`` is in an interactive session on a compute node.
Things to note about working in that context:

**Pros:**
  * You get dedicated access to cores on a compute node.
  * You can request multiple cores which improves the performance of basic :command:`jupyter lab` sessions, and opens up the possibility of doing things like setting up an interactive :program:`dask` cluster.
**Cons:**
  * You have to request an interactive compute node session for a set period of time with :command:`salloc` and wait for the session to start.
  * When the time requested for your session runs out,
    the session shutdown with no warning.


.. _JupyterComputeCanadaPythonVenv:

Create a Python Virtual Environment
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The first step is to create a Python virtual environment with ``jupyterlab``
(and probably other Python packages)
installed in it.

.. note::
    You don't have to create a new virtual environment every time you want to run :command:`jupyter lab`.
    Just be sure to activate your virtual environment before you launch :command:`jupyter lab`.

Python virtual environments (venvs) are similar to :program:`conda` environments in that they facilitate isolated installation and management of Python packages in a repeatable way.
Although :program:`conda` packages are MOAD's preferred tool for package isolation,
Compute Canada `explicitly stipulates`_ that we should not use Anaconda or :program:`conda` environments on their clusters.
So,
this section describes how to use a Python venv to install and run :command:`jupyter lab`.

.. _explicitly stipulates: https://docs.alliancecan.ca/wiki/Anaconda/en

Use the Compute Canada module system to load Python,
preferably the most recent available version.
On ``graham`` in Dec-2022 that is Python 3.10.2:

.. code-block:: bash

    $ module load python/3.10.2

Create a Python virtualenv in which to install ``jupyterlab`` and other packages:

.. code-block:: bash

    $ python3 -m virtualenv --no-download ~/venvs/jupyter

The ``--no-download`` forces the ``pip``,
``setuptools``,
and ``wheel`` packages to be installed from the package collections
(also known as "wheelhouses")
maintained by Compute Canada.
This virtual environment will be created in the :file:`$HOME/venvs/jupyter/`.
:program:`virtualenv` takes care of creating all of the necessary directories.

Activate the venv with:

.. code-block:: bash

    $ source ~/venvs/jupyter/bin/activate

The name of the venv will be prepended in parentheses to your command prompt:
``(jupyter)``,
in this case.

Update the version of :program:`pip` installed in the venv.
This rarely seems to have any effect,
but it is recommended in the `Compute Canada venv docs`_,
so we do it:

.. code-block:: bash

    (jupyter)$ python3 -m pip install --no-index --upgrade pip

.. _Compute Canada venv docs: https://docs.alliancecan.ca/wiki/Python#Creating_and_using_a_virtual_environment

Install the ``jupyterlab`` package and other packages that we routinely use for analysis into the venv:

.. code-block:: bash

    (jupyter)$ python3 -m pip install jupyterlab xarray h5netcdf bottleneck matplotlib cmocean

This will cause the list of packages ``jupyterlab xarray h5netcdf bottleneck matplotlib cmocean``
to be installed from the package collections maintained by Compute Canada,
or from the `Python Package Index (PyPI)`_.
Ideally all of the packages will be installed from the Compute Canada package collections,
ensuring that they have been built for best compatibility and optimization for the cluster architecture.
However,
when packages are unavailable or not up to date in the Compute Canada collections,
they are installed from PyPI.

.. _Python Package Index (PyPI): https://pypi.org/

.. important::
    In late Sep-2021 we discovered that the ``netCDF4`` package maintained by Compute Canada had become incompatible with ``xarray``
    (then at version 0.19.0).
    The work-around is to change to use the `h5netcdf package`_ to access netCDF files.
    The :command:`python3 -m pip install ...` command above will install ``h5netcdf``.
    To use it,
    add a ``engine="h5netcdf"`` argument to your ``xarray.open_dataset()``,
    ``xarray.Dataset.to_netcdf()``,
    etc. calls.

    If you want to use ``h5netcdf`` at a lower level than ``xarray``
    (as you may have used ``netCDF4`` elsewhere),
    please see its legacy API that is designed for compatibility with ``netCDF4``.

    .. _h5netcdf package: https://github.com/h5netcdf/h5netcdf

.. note::
    If you need to deactivate the venv,
    perhaps to activate a venv with a different collection of packages installed,
    use:

    .. code-block:: bash

        (jupyter)$ deactivate


.. _JupyterComputeCanadaInteractiveCompute:

Running :command:`jupyter lab` in an Interactive Compute Session
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

In an :program:`ssh` session on ``graham``,
start an interactive session on a compute node with:

.. code-block:: bash

    $ salloc --time=1:00:00 --ntasks=1 --cpus-per-task=2 --mem-per-cpu=1024M --account=rrg-allen

The ``--time=1:00:00`` option requests the compute node resources for 1 hour.
``--ntasks=1 --cpus-per-task=2 --mem-per-cpu=1024M`` requests 2 cores with 1024 Mb of RAM each for the session and associates them with 1 scheduler task.
Those are good choices for typical interactive work on NEMO results files.
The ``--account=rrg-allen`` uses the MOAD allocation on ``graham`` to request the resources with better than default priority.
On other clusters use ``--account=def-allen``.

You should see output something like:

.. code-block:: text

    salloc: Pending job allocation 40482784
    salloc: job 40482784 queued and waiting for resources
    salloc: job 40482784 has been allocated resources
    salloc: Granted job allocation 40482784
    salloc: Waiting for resource configuration
    salloc: Nodes gra705 are ready for job

as the requested session starts up.
There may be a wait while the resources are allocated to you,
depending on how busy the cluster is,
how long a session you have requested,
how many cores you have requested,
and how much memory you have requested.
Eventually,
your command-line prompt should re-appear showing that you are now connected to one of the compute nodes,
``gra705`` in this case:

.. code-block:: bash

    [your-user-id@gra705 ~]$


Load a Compute Canada Python language module,
and activate the Python virtual environment in which ``jupyterlab`` and the other packages that you need are installed.
In this example we load Python 3.8.2 and activate our environment from the :file:`~/venvs/jupyter/` directory:

.. code-block:: bash

    $ module load python/3.8.2
    $ source ~/venvs/jupyter/bin/activate

Navigate to the directory that you want to be at the top level of Jupyter's file navigation,
and start the Jupyter server.
For example,
if you are working in your analysis repo,
the commands would be like:

.. code-block:: bash

    (jupyter) [dlatorne@gra581 ~]$ cd $PROJECT/MEOPAR/analysis-doug/
    (jupyter) [dlatorne@gra581 ~]$ jupyter lab --no-browser --ip $(hostname -f)

The ``--no-browser`` option in that command tells :program:`jupyter` to start the server part only,
and not to start the client part in a browser.
The ``--ip $(hostname -f)`` causes the name of the node you are running the server on to be used in the URLs that Jupyter sets up for the server.

You should see output in that terminal window that looks something like:

.. code-block:: text

    [I 17:26:04.998 LabApp] Writing notebook server cookie secret to /home/dlatorne/.local/share/jupyter/runtime/notebook_cookie_secret
    [I 17:26:07.186 LabApp] JupyterLab extension loaded from /home/dlatorne/venvs/jupyter/lib/python3.8/site-packages/jupyterlab
    [I 17:26:07.186 LabApp] JupyterLab application directory is /home/dlatorne/venvs/jupyter/share/jupyter/lab
    [I 17:26:07.191 LabApp] Serving notebooks from local directory: /home/dlatorne/projects/def-allen/dlatorne/MEOPAR/analysis-doug/
    [I 17:26:07.191 LabApp] Jupyter Notebook 6.1.5 is running at:
    [I 17:26:07.191 LabApp] http://gra705.graham.sharcnet:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036
    [I 17:26:07.191 LabApp]  or http://127.0.0.1:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036
    [I 17:26:07.191 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
    [C 17:26:07.203 LabApp]

        To access the notebook, open this file in a browser:
            file:///home/dlatorne/.local/share/jupyter/runtime/nbserver-24995-open.html
        Or copy and paste one of these URLs:
            http://gra705.graham.sharcnet:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036
         or http://127.0.0.1:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036

.. note::
    Keep this terminal window open.
    It is where the Jupyter server part is running.
    If you close it,
    you will shutdown the Jupyter server and your :command:`jupyter lab` session will stop working.

The URLs on the last 2 lines are the important bit that we need to use to get the client running on our laptop.
The second last one that contains the name of the node that the server is running on is the important one for the rest of this setup.
That is::

  http://gra705.graham.sharcnet:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036

in the example output above.

The ``gra705.graham.sharcnet`` part is the name of the compute node on which your Jupyter server is running.
It will change from session to session.
The number after ``gra705.graham.sharcnet:`` in the URL
(``8888`` above)
is the port number that the Jupyter server is running on.
``8888`` is the default,
but if that port is busy,
probably because somebody else is already running a Jupyter server on it,
Jupyter will choose a different port number.
You need to use the port number that *your* Jupyter server server is running on in the next step when we set up the :program:`ssh` tunnel between your laptop and ``graham`` for the Jupyter client to use.

To set up the :program:`ssh` tunnel,
open a new terminal window on your laptop,
and enter the command:

.. code-block:: bash

    $ ssh -N -L 4343:gra705.graham.sharcnet:8888 graham

This use of :program:`ssh` is called "port forwarding", or "ssh tunnelling".
It creates an ssh encrypted connection between a port on your laptop
(port ``4343`` in this case)
and a port on the remote host
(port ``8888`` on the ``gra705.graham.sharcnet`` node in this case).
The ``-N`` option tells :program:`ssh` not to execute a command on the remote system because all we want to do is set up the port forwarding.
The ``-L`` option tells :program:`ssh` that the next blob of text is the details of the port forwarding to set up.

You can use any number ``≥1024`` you want instead of ``4343`` as the local port number on your laptop.
The number after ``:gra705.graham.sharcnet:`` has to be the same as the port number in the URLs that the Jupyter server printed out.

.. note::
    Keep this terminal window open too.
    If you close it,
    you will collapse the :program:`ssh` port forwarding tunnel and your Jupyter server and client will stop being able to talk to each other.

Finally,
open a new tab in the browser on your laptop and go to ``http://localhost:4343/`` to bring up the Jupyter client.
Use whatever port number you chose,
if you chose to use something other than ``4343`` in the :command:`ssh -N -L ...` command.
You may land on a Jupyter page that asks you to enter a :guilabel:`Password or token` to log in.
If so,
copy the the long string of digits and letters from the URL in the Jupyter server terminal windows.
For example,
the in the URL::

  http://gra705.graham.sharcnet:8888/?token=327caed3d832eefaad25a57cbf01de9f42685ced4306e036

the token is ``327caed3d832eefaad25a57cbf01de9f42685ced4306e036``.

When you run Jupyter in this way,
remember that all of the notebooks and files you are working with are on the remote computer (``graham``) file system,
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
