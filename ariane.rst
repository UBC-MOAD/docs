.. Copyright 2018 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   http://creativecommons.org/licenses/by/4.0/


.. Ariane-docs:

******
Ariane
******

`Ariane`_ is a particle tracking software that can be applied to NEMO and ROMS model NetCDF results files to trace the source of water masses (running backward in time) or to calculate where the water packet goes to (running forward in time).

.. _Ariane: http://stockage.univ-brest.fr/~grima/Ariane/whatsariane.html

References:
-----------

* Manual: `Compilation and Installation`_
* Manual: `Ariane Namelist`_
* Manual: `Ariane Tutorial`_
* Blanke, B., and S. Raynaud, 1997: Kinematics of the Pacific Equatorial Undercurrent: a Eulerian and Lagrangian approach from GCM results. J. Phys. Oceanogr., 27, 1038-1053.
* Blanke, B., M. Arhan, G. Madec, and S. Roche, 1999: Warm water paths in the equatorial Atlantic as diagnosed with a general circulation model. J. Phys. Oceanogr., 29, 2753-2768.
* Blanke, B., S. Speich, G. Madec, and K. Döös, 2001: A global diagnostic of interocean mass transfers. J. Phys. Oceanogr., 31, 1623-1632.
.. _Compilation and Installation: http://stockage.univ-brest.fr/~grima/Ariane/ariane_install_2.x.x_sep08.pdf
.. _Ariane Namelist: http://stockage.univ-brest.fr/~grima/Ariane/ariane_namelist_2.x.x_oct08.pdf
.. _Ariane Tutorial: http://stockage.univ-brest.fr/~grima/Ariane/ariane_tutorial_2.x.x_sep08.pdf


.. _GettingAriane:

Getting Ariane
==============

The MOAD group maintains our own Mercurial repository on Bitbucket of the Ariane code base; this repository is accessible only by members of the MOAD group so as to respect the sign-up requirement of the upstream Ariane repository.

Installing on :kbd:`salish`
--------------------------------

Specify the locations of the :kbd:`netcdf` libraries to help the :kbd:`configure` script find them:

.. code-block:: bash

        cd /ocean/$USER/$PROJECT/ariane-2.2.8_04/
    export NETCDF_INC=/usr/include
    export NETCDF_LIB=/usr/lib

Configure the installation by going to the folder of your downloaded Ariane code from their website:

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.2.8_04
        ./configure --prefix=/ocean/$USER/$PROJECT/ariane-2.2.8_04

The :kbd:`prefix` argument overwrites the default install directory into a customized directory.

Make and install Ariane (you will need to do this every time you make changes to the code):

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.2.8_04
        make
        make check
        make install

:kbd:`make` compiles source files, :kbd:`make check` tests Ariane's qualitative and quantitative modes, and :kbd:`make install` installs Ariane.

Add the path for the Ariane executable to your :kbd:`PATH` environment variable:

.. code-block:: bash

        export PATH=/ocean/$USER/$PROJECT/ariane-2.2.8_04/bin:$PATH

Now you can run Ariane from any directory by typing :kbd:`ariane`.

Testing Ariane installation
---------------------------

To test that you have everything set up correctly, run one of the Ariane examples.
For instance, try:

.. code-block:: bash

        cd /ocean/$USER/$PROJECT/ariane-2.2.8_04/examples/qualitative
    ariane

You should notice several new files, such as :kbd:`ariane_trajectories_qualitative.nc` and :kbd:`traj.txt`.
These files contain the trajectory information.

* :kbd:`ariane_trajectories_qualitative.nc` can be loaded into a notebook to plot the particle locations over time and starting/finishing points, etc.
* :kbd:`traj.txt` is helpful if you want to get a general idea of what the resulting trajectory coordinates look like or to check if the simulation ran properly.

.. _Configuringyourrun:

Configuring your run
====================

insert text here

.. _Analyzing_output:

Analyzing output
================================

insert text here
