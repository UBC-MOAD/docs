.. Copyright 2018-2019 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   http://creativecommons.org/licenses/by/4.0/


.. _ComputeCanadaDocs:

**********************************
Working on Compute Canada Clusters
**********************************

`Compute Canada`_ is the organization that coordinates access to High Performance Computing (HPC) computing resources across Canada.

.. _Compute Canada: https://www.computecanada.ca/

Before you can use Compute Canada compute clusters and high capacity storage resources you need to :ref:`ComputeCanadaAccount`.
You only need to do that once when you join the MOAD group,
no matter how many different compute clusters you end up working on.

For each cluster that you work on,
you need to do some initial setup.
There are instructions below for each cluster that we use:

* :kbd:`beluga.computecanada.ca`,
  located in Montréal,
  see


.. _ComputeCanadaAccount:

Create Compute Canada & WestGrid Accounts
=========================================

WestGrid is a computing resource shared by western Canadian universities. We use WestGrid for computationally intensive simulations. The large number of processors available often reduces simulation run-time considerably.

Compute Canada is an organization coordinating access to computing resources provided by WestGrid and three other regional HPC consortia (Compute Ontario, Calcul Québec, and ACENET).

In order to use those resources you need to create Compute Canada and WestGrid accounts.
To create those accounts follow the steps on this page:

https://www.westgrid.ca/support/accounts/registering_ccdb .

.. note::
   When prompted to select an institution, choose :kbd:`WestGrid: University of British Columbia`.

   If you are creating an account as a sponsored user ask your supervisor for their CCRI code.


.. _InitialSetupOnBeluga:

Initial Setup on :kbd:`beluga.computecanada.ca`
===============================================

These are the setup steps that you need to do when you start using :kbd:`beluga` for the first time:

#. Add an entry for :kbd:`beluga` to your :file:`$HOME/.ssh/config` file.
   This will enable you to connect to :kbd:`beluga` by typing :command:`ssh beluga` instead of
   having to type :command:`ssh your-user-id@beluga.computecanada.ca`.

   Create a :file:`$HOME/.ssh/config` file on your Waterhole machine containing the following
   (or append the following if :file:`$HOME/.ssh/config` already exists)::

       Host beluga
         Hostname  beluga.computecanada.ca
         User  userid
         ForwardAgent  yes

   where :kbd:`userid` is your Compute Canada user id.

   The first two lines establish :kbd:`beluga` as a short alias for :kbd:`beluga.computecanada.ca` so that you can just type :command:`ssh beluga`.

   The third line sets the user id to use on :kbd:`beluga`,
   which is convenient if it differs from your EOAS user id.

   The last line enables agent forwarding so that authentication requests received on the remote system are passed back to your Waterhole machine for handling.
   That means that connections to Bitbucket (for instance) in your session on :kbd:`beluga` will be authenticated by your Waterhole machine.
   So,
   after you type your :command:`ssh` key passphrase into your Waterhole machine once,
   you should not have to type it again until you log off and log in again.

#. Copy your :command:`ssh` public key into your :file:`$HOME/.ssh/authorized_keys` file on :kbd:`beluga` and set the permissions on that file so that only you can read, write, or delete it.
   The :command:`copy-ssh-id` command makes that a lot easier than it sounds:

   .. code-block:: bash

       $ ssh-copy-id -i $HOME/.ssh/id_rsa beluga

   You should see output like
   (except that :kbd:`/home/dlatorne/.ssh/id_rsa.pub` in the 1st line should show your EOAS user id,
   not Doug's)::

      /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/dlatorne/.ssh/id_rsa.pub"
      The authenticity of host 'beluga.computecanada.ca (132.219.136.2)' can't be established.
      ECDSA key fingerprint is SHA256:9tRJeRU3sa45G4oxtJxwLo7rPIjWwGaogVwushkvFtE.
      Are you sure you want to continue connecting (yes/no)?

   Type :kbd:`yes` to accept the fingerprint from :kbd:`beluga`.
   Then you should see output like
   (again with your user id, not Doug's)::

     /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/dlatorne/.ssh/id_rsa.pub"
     /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
     /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
     dlatorne@beluga.computecanada.ca's password:

   Type in your Compute Canada password.
   The output should continue with::

     intel/2018.3:
     ============================================================================================
     The software listed above is available for non-commercial usage only. By
     continuing, you
     accept that you will not use the software for commercial purposes.

     Le logiciel listé ci-dessus est disponible pour usage non commercial
     seulement. En
     continuant, vous acceptez de ne pas l'utiliser pour un usage commercial.
     ============================================================================================


     Number of key(s) added: 1

     Now try logging into the machine, with:   "ssh beluga"
     and check to make sure that only the key(s) you wanted were added.

   Finally,
   as the output above suggests,
   confirm that you can :command:`ssh` into :kbd:`beluga` with

   .. code-block:: bash

       $ ssh beluga

   No userid, password, or lengthy host name required! :-)

#. Create a :envvar:`PROJECT` environment variable that points to our allocated storage on the :file:`/project/` file system.
   To ensure that :envvar:`PROJECT` is set correctly every time you sign in to :kbd:`beluga`,
   use an editor to add the following line to your :file:`$HOME/.bash_profile` file::

     export PROJECT=$HOME/projects/def-allen

   Exit your session on :kbd:`beluga` with :command:`exit`,
   then :command:`ssh` in again,
   and confirm that :envvar:`PROJECT` is set correctly with:

   .. code-block:: bash

       $ echo $PROJECT

   The output should be::

     /home/dlatorne/projects/def-allen/

   except with your Compute Canada userid instead of Doug's.

#. Set the permissions in your :file:`$PROJECT/$USER/` directory so that other members of the :kbd:`def-allen` group have access,
   and permissions from the top-level directory are inherited downward in the tree:

   .. code-block:: bash

       $ cd $PROJECT/$USER
       $ chmod g+rwxs .
       $ chmod o+rx .

   Check the results of those operations with :command:`ls -al $PROJECT/$USER`.
   They should look like::

     $ ls -al $PROJECT/$USER
     total 90
     drwxrwsr-x  3 dlatorne def-allen 33280 Apr  9 15:04 ./
     drwxrws--- 16 allen    def-allen 33280 Apr  8 18:14 ../

   with your user id instead of Doug's in the :file:`./` line.

#. Set the group and permissions in your :file:`$SCRATCH/` directory so that other members of the :kbd:`def-allen` group have access,
   and permissions from the top-level directory are inherited downward in the tree:

   .. code-block:: bash

       $ cd $SCRATCH
       $ chgrp def-allen .
       $ chmod g+rwxs .
       $ chmod o+rx .

   Check the results of those operations with :command:`ls -al $SCRATCH`.
   They should look like::

     $ ls -al $SCRATCH
     total 3015
     drwxrwsr-x    26 dlatorne def-allen   41472 Apr 26 17:23 ./
     drwxr-xr-x 16366 root     root      2155008 Apr 29 15:31 ../

   with your user id instead of Doug's in the :file:`./` line.

#. Follow the :ref:`MercurialConfiguration` docs to create your :file:`$HOME/.hgrc` Mercurial configuration file,
   and your :file:`$HOME/.hgignore` global ignore file for Mercurial.

#. Compute Canada clusters use the :command:`module load` command to load software components.
   On :kbd:`beluga` the module loads that are required to build and run NEMO are:

.. code-block:: bash

    module load netcdf-fortran-mpi/4.4.4
    module load perl/5.22.4
    module load python/3.7.0

You can manually load the modules each time you log in,
or you can add the above lines to your :file:`$HOME/.bashrc` file so that they are automatically loaded upon login.

#. Follow the docs for the project that you are working on to set up your :file:`$PROJECT/$USER/` workspace and clone the repositories required to build and run NEMO:

   * For the MOEPAR SalishSeaCast project,
     follow the :ref:`salishseadocs:CreateWorkspaceAndCloneRepositories` and then the :ref:`salishseadocs:InstallCommandProcessorPackages` docs

   If you have trouble with cloning of some repositories repeatedly timing out,
   see the :ref:`WorkAroundForCloningTimeout` section of the :ref:`vc-with-hg` docs
