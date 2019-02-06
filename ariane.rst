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

Ariane can be run in two modes: quantitative and qualitative. For the rest of the documentation below, we are running in qualitative mode. 

References

* Manual: `Compilation and Installation`_
* Manual: `Ariane Namelist`_
* Manual: `Ariane Tutorial`_
* Blanke, B., and S. Raynaud, 1997: Kinematics of the Pacific Equatorial Undercurrent: a Eulerian and Lagrangian approach from GCM results. J. Phys. Oceanogr., 27, 1038-1053.
* Blanke, B., M. Arhan, G. Madec, and S. Roche, 1999: Warm water paths in the equatorial Atlantic as diagnosed with a general circulation model. J. Phys. Oceanogr., 29, 2753-2768.
* Blanke, B., S. Speich, G. Madec, and K. Döös, 2001: A global diagnostic of interocean mass transfers. J. Phys. Oceanogr., 31, 1623-1632.

.. _Compilation and Installation: http://stockage.univ-brest.fr/~grima/Ariane/ariane_install_2.x.x_sep08.pdf
.. _Ariane Namelist: http://stockage.univ-brest.fr/~grima/Ariane/ariane_namelist_2.x.x_oct08.pdf
.. _Ariane Tutorial: http://stockage.univ-brest.fr/~grima/Ariane/ariane_tutorial_2.x.x_sep08.pdf

.. _Getting Ariane:

Getting Ariane
==============

The MOAD group maintains our own Mercurial repository on Bitbucket of the Ariane code base; this repository is accessible only by members of the MOAD group so as to respect the sign-up requirement of the upstream Ariane repository. The general Ariane code is available via the `Ariane website`_ . Modifications made by the MOAD group to the Ariane source code can be found on `Bitbucket`_. To add this to your own Ariane installation, clone the repository from Bitbucket to your $PROJECT space and replace the changed files in your Ariane source folder.

.. _Ariane website: http://stockage.univ-brest.fr/~grima/Ariane/download.php
.. _Bitbucket: http://www.bitbucket.org/eliseolsen/arianesrc-2.2.8_04

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/
    hg clone ssh://hg@bitbucket.org/eliseolsen/arianesrc-2.2.8_04


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

You should notice several new files, such as :file:`ariane_trajectories_qualitative.nc` and :file:`traj.txt`.
These files contain the trajectory information.

* :file:`ariane_trajectories_qualitative.nc` can be loaded into a notebook to plot the particle locations over time and starting/finishing points, etc.
* :file:`traj.txt` is helpful if you want to get a general idea of what the resulting trajectory coordinates look like or to check if the simulation ran properly.

.. _Configuring your run:

Configuring your run
====================

:kbd:`intitial_positions.txt`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The :file:`initial_positions.txt` file specifies the initial positions and initial times of the particles that you are tracking. This file consists of 5 columns and a row for each particle that you are running.

.. note::

    Ariane uses FORTAN indexing, which counts starting at 1. If you used Python to look up initial positions, you should add 1 to your initial positions.


The first three columns represent the initial X, Y, and Z coordinate point of your particle. A negative Z coordinate tells Ariane to confine the particle to its original depth throughout the trajectory. Note that these coordinate points need to be offset by 0.01, otherwise Ariane struggles at the boundaries between two grid boxes. The fourth column is the time index (use 0.5 if you want to start at NEMO time 00:00). The last column parameter is always set to 1.0.
Here is an example :file:`initial_positions.txt` file:

.. code-block:: text

    310.01 360.01 5.0  0.5 1.0
    310.01 360.01 10.0 0.5 1.0
    310.01 400.01 5.0  0.5 1.0
    310.01 400.01 10.0 0.5 1.0
    310.01 400.01 15.0 0.5 1.0

:kbd:`namelist`
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
The :file:`namelist` file specifies 


.. _Analyzing output:

Analyzing output
================================

The NetCDF file that contains the particle tracks is named :file:`ariane_trajectories_qualitative.nc`. The variables in this file include the initial and final x, y, z, and time for the particles. It is a good idea to double check that these agree with the locations you listed in :file:`initial_positions.txt`. To plot and analyze the output, you will generally want to read in traj_lon, traj_lat, traj_depth, and traj_time. These variables have the shape (number of particles, positions in time). The output can look something like this:

If you would like to see some examples of particle tracking, feel free to look at the following notebooks:

* `ParticleTracking.ipynb`_

.. _ParticleTracking.ipynb: https://nbviewer.jupyter.org/urls/bitbucket.org/salishsea/analysis/raw/tip/Idalia/ParticleTracking.ipynb

