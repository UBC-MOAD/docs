.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _AllianceDocs:

*******************************************************
Working on Digital Research Alliance of Canada Clusters
*******************************************************

The `Digital Research Alliance of Canada`_,
or the Alliance,
(formerly Compute Canada) is the organization that coordinates access to High Performance Computing
(HPC) computing resources across Canada.

.. _Digital Research Alliance of Canada: https://alliancecan.ca/en

Before you can use the Alliance compute clusters and high capacity storage resources you need to
:ref:`CreateAllianceAccount`.
You only need to do that once when you join the MOAD group,
no matter how many different compute clusters you end up working on.

For each cluster that you work on,
you need to do some initial setup.
Our Alliance allocation that gives us higher than default priority for compute,
and larger project and nearline storage allocations is on the ``graham.computecanada.ca``
cluster located in Waterloo.
The instructions below are for setup on ``graham``.

We also have default allocations available on:

* ``beluga.computecanada.ca``,
  located in Montréal.
* ``cedar.computecanada.ca``,
  located in Burnaby.
* ``narval.computecanada.ca``,
  located in Montréal.

Our jobs generally do not require sufficient resources to qualify to run on the
``niagara.computecanada.ca`` cluster located in Toronto.


.. _CreateAllianceAccount:

Create Digital Research Alliance of Canada Account
==================================================

`Digital Research Alliance of Canada`_ (the Alliance) is the national network of shared
advanced research computing (ARC) and storage that we use for most of our ocean model calculations.
The BC DRI Group is the regional organization that coordinates the British Columbia partnership with the Alliance.

To access Alliance compute clusters and storage you need to register a Alliance account at
https://ccdb.alliancecan.ca/account_application.
To do so you will need an ``eoas.ubc.ca`` email address,
and Susan's Alliance CCRI code.

.. note::
   When prompted to select an institution, choose :guilabel:`BC DRI Group: University of British Columbia`.

There are detailed information about the account creation process,
and step by step instructions
(with screenshots)
for completing it at
https://alliancecan.ca/en/services/advanced-research-computing/account-management/apply-account


.. _InitialSetupOnGraham:

Initial Setup on ``graham.computecanada.ca``
============================================

These are the setup steps that you need to do when you start using ``graham`` for the first time:

#. Add an entry for ``graham`` to your :file:`$HOME/.ssh/config` file.
   This will enable you to connect to ``graham`` by typing :command:`ssh graham` instead of
   having to type :command:`ssh your-user-id@graham.computecanada.ca`.

   Create a :file:`$HOME/.ssh/config` file on your laptop or a Waterhole machine containing
   the following
   (or append the following if :file:`$HOME/.ssh/config` already exists):

   .. code-block:: text

       Host graham
         Hostname  graham.computecanada.ca
         User  userid
         ForwardAgent  yes

   where ``userid`` is your Alliance user id.

   The first two lines establish ``graham`` as a short alias for ``graham.computecanada.ca``
   so that you can just type :command:`ssh graham`.

   The third line sets the user id to use on ``graham``,
   which is convenient if it differs from your EOAS user id.

   The last line enables agent forwarding so that authentication requests received on the
   remote system are passed back to your laptop or Waterhole machine for handling.
   That means that connections to GitHub (for instance) in your session on ``graham``
   will be authenticated by your laptop or Waterhole machine.
   So,
   after you type your :command:`ssh` key passphrase into your laptop or Waterhole machine once,
   you should not have to type it again until you log off and log in again.

#. Add an entry for the data transfer nodes on ``graham`` to your :file:`$HOME/.ssh/config` file.
   The data transfer nodes are optimized for file transfers to and from the cluster.

   .. code-block:: text

       Host graham-dtn
         HostName  gra-dtn1.computecanada.ca
         User  userid
         ForwardAgent  no

   where ``userid`` is your Alliance user id.

#. Follow the Alliance docs to install your :command:`ssh` `public key into into the CCDB system`_
   so that it will be available to give you access to all of the Alliance HPC clusters.
   On Mac or Linux your public key is stored in ``$HOME/.ssh/id_ed25519.pub`` and you can display
   it so that you can copy/paste it to CCDB with:

   .. code-block:: bash

        cat $HOME/.ssh/id_ed25519.pub

   On Windows you can do that with:

   .. code-block:: powershell

        type %USERPROFILE%/.ssh/id_ed25519.pub

   Alternatively,
   you can open your :file:`id_ed25519.pub` in VS Code and copy it from there to the CCDB page.

   .. _public key into into the CCDB system: https://docs.alliancecan.ca/wiki/SSH_Keys#Using_CCDB

   Confirm that you can :command:`ssh` into ``graham`` with

   .. code-block:: bash

       $ ssh graham

   No userid, password, or lengthy host name required! :-)

#. Create a :envvar:`PROJECT` environment variable that points to our allocated storage on the
   :file:`/project/` file system.
   To ensure that :envvar:`PROJECT` is set correctly every time you sign in to ``graham``,
   use an editor to add the following line to your :file:`$HOME/.bash_profile` file:

   .. code-block:: text

       export PROJECT=$HOME/projects/def-allen

   Exit your session on ``graham`` with :command:`exit`,
   then :command:`ssh` in again,
   and confirm that :envvar:`PROJECT` is set correctly with:

   .. code-block:: bash

       $ echo $PROJECT

   The output should be:

   .. code-block:: text

       /home/dlatorne/projects/def-allen/

   except with your Alliance userid instead of Doug's.

#. Set the permissions in your :file:`$PROJECT/$USER/` directory so that other members of the
   ``def-allen`` group have access,
   and permissions from the top-level directory are inherited downward in the tree:

   .. code-block:: bash

       $ chmod g+rwxs $PROJECT/$USER
       $ chmod o+rx $PROJECT/$USER

   Check the results of those operations with :command:`ls -al $PROJECT/$USER`.
   They should look like:

   .. code-block:: text

       $ ls -al $PROJECT/$USER
       total 90
       drwxrwsr-x  3 dlatorne def-allen 33280 Apr  9 15:04 ./
       drwxrws--- 16 allen    def-allen 33280 Apr  8 18:14 ../

   with your user id instead of Doug's in the :file:`./` line.

#. Set the group and permissions in your :file:`$SCRATCH/` directory so that other members
   of the ``def-allen`` group have access,
   and permissions from the top-level directory are inherited downward in the tree:

   .. code-block:: bash

       $ chgrp def-allen $SCRATCH
       $ chmod g+rwxs $SCRATCH
       $ chmod o+rx $SCRATCH

   Check the results of those operations with :command:`ls -al $SCRATCH`.
   They should look like:

   .. code-block:: text

        $ ls -al $SCRATCH
        total 3015
        drwxrwsr-x    26 dlatorne def-allen   41472 Apr 26 17:23 ./
        drwxr-xr-x 16366 root     root      2155008 Apr 29 15:31 ../

   with your user id instead of Doug's in the :file:`./` line.

#. Follow the :ref:`GitConfiguration` docs to create your :file:`$HOME/.gitconfig` Git configuration file.

#. Alliance clusters use the :command:`module load` command to load software components.
   On ``graham`` the module loads that are required to build and run NEMO are:

   .. code-block:: bash

       module load StdEnv/2020
       module load netcdf-fortran-mpi/4.6.0
       module load perl/5.30.2

   You can manually load the modules each time you log in,
   or you can add the above lines to your :file:`$HOME/.bashrc` file so that they are
   automatically loaded upon login.

#. Follow the :ref:`salishseadocs:CreateWorkspaceAndCloneRepositories` docs to set up your
   :file:`$PROJECT/$USER/MEOPAR/` workspace and clone the repositories required to build
   and run NEMO.

#. Follow the :ref:`salishseadocs:InstallCommandProcessorPackages` docs to install the
   :ref:`salishseacmd:SalishSeaCmdProcessor` and its dependencies in a
   :command:`conda`  environment.

#. Follow the :ref:`BuildXIOS-MEOPAR-graham` docs to build ``XIOS-2``.

#. Follow the :ref:`salishseadocs:CompileNEMO-3.6-graham` docs to build ``NEMO-3.6``.

#. If you are using VS Code as your editor,
   consider setting up the :ref:`FortranLanguageServer`
