.. Copyright 2018-2019 The UBC EOAS MOAD Group
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

Ariane can be run in two modes: quantitative and qualitative. In quantitative mode, you release particles and end up with a distribution function for each grid cell quantifying where your particles end up and the mass transfer, while in qualitative mode you specify each of the particles that you release and trace their exact track. For the rest of the documentation below, we are running in qualitative mode.

References

* `Compilation and Installation`_
* `Ariane Namelist`_
* `Ariane Tutorial`_

.. _Compilation and Installation: http://stockage.univ-brest.fr/~grima/Ariane/ariane_install_2.x.x_sep08.pdf
.. _Ariane Namelist: http://stockage.univ-brest.fr/~grima/Ariane/ariane_namelist_2.x.x_oct08.pdf
.. _Ariane Tutorial: http://stockage.univ-brest.fr/~grima/Ariane/ariane_tutorial_2.x.x_sep08.pdf

.. _Getting Ariane:

Getting Ariane
==============

The MOAD group maintains our own Mercurial repository on Bitbucket of the Ariane code base; this repository is accessible only by members of the MOAD group so as to respect the sign-up requirement of the upstream Ariane repository. The general Ariane code is available via the `Ariane website`_ . Modifications made by the MOAD group to the Ariane source code can be found on `Bitbucket`_. To download the MOAD Ariane code base, clone the repository from Bitbucket to your $PROJECT space:

.. _Ariane website: http://stockage.univ-brest.fr/~grima/Ariane/download.php
.. _Bitbucket: http://www.bitbucket.org/UBC_MOAD/ariane-2.2.8-code

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/
    hg clone ssh://hg@bitbucket.org/UBC_MOAD/ariane-2.2.8-code


Installing on :kbd:`salish`
--------------------------------

Specify the locations of the :kbd:`netcdf` libraries to help the :kbd:`configure` script find them:

.. code-block:: bash

        cd /ocean/$USER/$PROJECT/ariane-2.2.8-code/
    export NETCDF_INC=/usr/include
    export NETCDF_LIB=/usr/lib

Configure the installation by going to the folder of your downloaded Ariane code from their website:

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.2.8-code
        ./configure --prefix=/ocean/$USER/$PROJECT/ariane-2.2.8-code

The :kbd:`prefix` argument overwrites the default install directory into a customized directory.

Make and install Ariane (you will need to do this every time you make changes to the code):

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.2.8-code
        make
        make check
        make install

Add the path for the Ariane executable to your :kbd:`PATH` environment variable:

.. code-block:: bash

        export PATH=/ocean/$USER/$PROJECT/ariane-2.2.8-code/bin:$PATH

Now you can run Ariane from any directory by typing :kbd:`ariane`.

Testing Ariane installation
---------------------------

To test that you have everything set up correctly, run one of the Ariane examples.
For instance, try:

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.2.8-code/examples/qualitative
    ariane

You should notice several new files, such as :file:`ariane_trajectories_qualitative.nc` and :file:`traj.txt`.
These files contain the trajectory information.

* :file:`ariane_trajectories_qualitative.nc` contains the particle positions at each time step and the initial positions
* :file:`traj.txt` gives a general idea of what the resulting trajectory coordinates look like or to check if the simulation ran properly

.. _Configuring your run:

Configuring your run
====================

:kbd:`intitial_positions.txt`
-----------------------------

The :file:`initial_positions.txt` file specifies the initial positions and release times of the particles that you are tracking. This file consists of 5 columns and a row for each particle that you are running.

.. note::

    Ariane uses FORTAN indexing, which counts starting at 1. If you used Python to look up initial positions, you should add 1 to your initial positions.

Within this file, the first three columns represent the initial X, Y, and Z coordinate point of your particle. A negative Z coordinate tells Ariane to confine the particle to its original depth throughout the trajectory. Note that these coordinate points should not be at the exact grid point coordinate, but rather offset by a little bit, otherwise Ariane may struggle at the boundaries between two grid boxes. The fourth column is the time index (use 0.5 if you want to start at NEMO time 00:00, if 0.0 it will interpolate between your data files), note that if you are running backwards, the time index here should be your end time step (so if you have a total of 330 time steps, you should release the particles at 329.5). The last column parameter is always set to 1.0.
Here is an example :file:`initial_positions.txt` file:

.. code-block:: text

    310.01 360.01 5.0  0.5 1.0
    310.01 360.01 10.0 0.5 1.0
    310.01 400.01 5.0  0.5 1.0
    310.01 400.01 10.0 0.5 1.0
    310.01 400.01 15.0 0.5 1.0

:kbd:`namelist`
---------------

The :file:`namelist` file specifies a variety of the run settings. The general Ariane parameters can be specified within :kbd:`Ariane`; the main ones that you are likely to change are:

+----------------------------------------+-------------------------------------------+
|    Parameter                           |              Description                  |
+========================================+===========================================+
| :kbd:`forback`                         | Operate Ariane 'forward' or 'backward'    |
+----------------------------------------+-------------------------------------------+
| :kbd:`nmax`                            | Number of particles that you trace        |
+----------------------------------------+-------------------------------------------+
| :kbd:`tunit`                           | Unit of time of your model files (sec)    |
+----------------------------------------+-------------------------------------------+
| :kbd:`ntfic`                           | Number of :kbd:`tunit` in each model file |
+----------------------------------------+-------------------------------------------+

The parameters of your model run are specified in :kbd:`OPAPARAM`:

+----------------------------------------+---------------------------------------------+
|    Parameter                           |              Description                    |
+========================================+=============================================+
| :kbd:`imt`, :kbd:`jmt`, :kbd:`kmt`     | x, y, and z dimensions of your model domain |
+----------------------------------------+---------------------------------------------+
| :kbd:`lmt`                             | Time dimension (total number of time steps) |
+----------------------------------------+---------------------------------------------+

In qualitative mode, the frequency of calculation of the trajectory and of writing to the output file is set within :kbd:`QUALITATIVE`:

+----------------------------------------+-----------------------------------------------------------------+
|    Parameter                           |              Description                                        |
+========================================+=================================================================+
| :kbd:`delta_t`                         | Time step size (seconds)                                        |
+----------------------------------------+-----------------------------------------------------------------+
| :kbd:`frequency`                       | Number of :kbd:`delta_t` to calculate                           |
+----------------------------------------+-----------------------------------------------------------------+
| :kbd:`nb_output`                       | Number of output time steps ( in units of delta_t x frequency)  |
+----------------------------------------+-----------------------------------------------------------------+

The parameters for reading in the U, V, and W velocity files are indicated in :kbd:`ZONALCRT`, :kbd:`MERIDCRT`, and :kbd:`VERTICRT`. The parameters are roughly the same, for example in the :kbd:`ZONALCRT` section:

+----------------------------------------+------------------------------------------------+
|    Parameter                           |              Description                       |
+========================================+================================================+
| :kbd:`c_dir_zo`                        | Directory where data is stored                 |
+----------------------------------------+------------------------------------------------+
| :kbd:`c_prefix_zo`                     | NetCDF file name with velocity data            |
+----------------------------------------+------------------------------------------------+
| :kbd:`nc_var_zo`                       | Variable name for velocity component           |
+----------------------------------------+------------------------------------------------+
| :kbd:`ind0_zo`                         | First number of file to read                   |
+----------------------------------------+------------------------------------------------+
| :kbd:`indn_zo`                         | Last number of file to read                    |
+----------------------------------------+------------------------------------------------+
| :kbd:`maxsize_zo`                      | Maximum number of integers in file name number |
+-----------------------------------------------------------------------------------------+

Note that even in backwards mode, the first and last number of the files to read are in the forwards direction, i.e. from 1 to your last file number. Of course this is not a comprehensive list of all the parameters you can set in the :file:`namelist`. More information can be found in the references listed at the start.

.. _Analyzing output:

Analyzing output
================================

The NetCDF file that contains the particle tracks is named :file:`ariane_trajectories_qualitative.nc`. The variables in this file include the initial and final x, y, z, and time for the particles. It is a good idea to double check that these agree with the locations you listed in :file:`initial_positions.txt`. To plot and analyze the output, you read in traj_lon, traj_lat, traj_depth, and traj_time. These variables have the shape (number of particles, positions in time).

If you would like to see some examples of particle tracking, feel free to look at the following notebooks:

* `ParticleTracking.ipynb`_

.. _ParticleTracking.ipynb: https://nbviewer.jupyter.org/urls/bitbucket.org/salishsea/analysis/raw/tip/Idalia/ParticleTracking.ipynb

