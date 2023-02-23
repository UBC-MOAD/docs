.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _Ariane-docs:

******
Ariane
******

`Ariane`_ is a particle tracking software that can be applied to NEMO and ROMS model NetCDF results files to trace the source of water masses (running backward in time) or to calculate where the water packet goes to (running forward in time).

.. _Ariane: http://stockage.univ-brest.fr/~grima/Ariane/whatsariane.html

Ariane can be run in two modes: quantitative and qualitative. In quantitative mode, you release particles and end up with a distribution function for each grid cell quantifying where your particles end up and the mass transfer, while in qualitative mode you specify each of the particles that you release and trace their exact track. For the rest of the documentation below, we are running in qualitative mode. Documentation on how to run `quantitative mode`_ is provided in the Salish Sea MEOPAR Read the Docs. 

.. _quantitative mode: https://salishsea-meopar-docs.readthedocs.io/en/latest/particles/quantitative.html

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

The MOAD group maintains our own Git repository on GitHub of the Ariane code base; this repository is accessible only by members of the MOAD group so as to respect the sign-up requirement of the upstream Ariane repository. The general Ariane code is available via the `Ariane website`_ . Modifications made by the MOAD group to the Ariane source code can be found on `GitHub`_. To download the MOAD Ariane code base, clone the repository from GitHub to your $PROJECT space:

.. _Ariane website: http://stockage.univ-brest.fr/~grima/Ariane/download.php
.. _GitHub: https://github.com/UBC-MOAD/ariane-2.3.0_03

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/
    git clone git@github.com:UBC-MOAD/ariane-2.3.0_03.git


Installing on ``salish``
------------------------

Configure the installation by going to the folder of your clone of the MOAD Ariane repo:

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.3.0_03
    ./configure --prefix=$PWD

The ``prefix`` argument overwrites the default install directory into a customized directory.

Make and install Ariane (you will need to do this every time you make changes to the code):

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.3.0_03
    make
    make check
    make install

Add the path for the Ariane executable to your ``PATH`` environment variable:

.. code-block:: bash

    export PATH=$PWD/bin:$PATH

Now you can run Ariane from any directory by typing ``ariane``.


Testing Ariane installation
---------------------------

To test that you have everything set up correctly, run one of the Ariane examples.
For instance, try:

.. code-block:: bash

    cd /ocean/$USER/$PROJECT/ariane-2.3.0_03/examples/qualitative
    ariane

You should notice several new files, such as :file:`results/ariane_trajectories_qualitative.nc` and :file:`validation/traj.txt`.
These files contain the trajectory information.

* :file:`results/ariane_trajectories_qualitative.nc` contains the particle positions at each time step and the initial positions
* :file:`validation/traj.txt` gives a general idea of what the resulting trajectory coordinates look like or to check if the simulation ran properly


.. _Configuring your run:

Qualitative Mode
================

:file:`intitial_positions.txt`
------------------------------

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


:file:`namelist`
----------------

The :file:`namelist` file specifies a variety of the run settings. The general Ariane parameters can be specified within ``Ariane``; the main ones that you are likely to change are:

+----------------------------------------+-------------------------------------------------------+
|    Parameter                           |              Description                              |
+========================================+=======================================================+
| ``forback``                            | Operate Ariane 'forward' or 'backward'                |
+----------------------------------------+-------------------------------------------------------+
| ``nmax``                               | Number of particles that you trace                    |
+----------------------------------------+-------------------------------------------------------+
| ``tunit``                              | Unit of time of your model files (sec)                |
+----------------------------------------+-------------------------------------------------------+
| ``ntfic``                              | Number of ``tunit`` in each time step (typically = 1) |
+----------------------------------------+-------------------------------------------------------+

The parameters of your model run are specified in ``OPAPARAM``:

+----------------------------------------+-------------------------------------------------------------------+
|    Parameter                           |                            Description                            |
+========================================+===================================================================+
| ``imt``, ``jmt``, ``kmt``              | x, y, and z dimensions of your model domain                       |
+----------------------------------------+-------------------------------------------------------------------+
| ``lmt``                                | Time dimension (total number of time steps)                       |
+----------------------------------------+-------------------------------------------------------------------+
| ``key_computew``                       | Set to 'TRUE' if the model you are using does not have 'w' output |
+----------------------------------------+-------------------------------------------------------------------+

In qualitative mode, the frequency of calculation of the trajectory and of writing to the output file is set within ``QUALITATIVE``:

+----------------------------------------+-----------------------------------------------------------------+
|    Parameter                           |              Description                                        |
+========================================+=================================================================+
| ``delta_t``                            | Time step size (seconds)                                        |
+----------------------------------------+-----------------------------------------------------------------+
| ``frequency``                          | Number of ``delta_t`` to calculate                              |
+----------------------------------------+-----------------------------------------------------------------+
| ``nb_output``                          | Number of output time steps ( in units of delta_t x frequency)  |
+----------------------------------------+-----------------------------------------------------------------+

The parameters for reading in the U, V, and W velocity files are indicated in ``ZONALCRT``, ``MERIDCRT``, and ``VERTICRT``. The parameters are roughly the same, for example in the ``ZONALCRT`` section:

+----------------------------------------+------------------------------------------------+
|    Parameter                           |              Description                       |
+========================================+================================================+
| ``c_dir_zo``                           | Directory where data is stored                 |
+----------------------------------------+------------------------------------------------+
| ``c_prefix_zo``                        | NetCDF file name with velocity data            |
+----------------------------------------+------------------------------------------------+
| ``nc_var_zo``                          | Variable name for velocity component           |
+----------------------------------------+------------------------------------------------+
| ``ind0_zo``                            | First number of file to read                   |
+----------------------------------------+------------------------------------------------+
| ``indn_zo``                            | Last number of file to read                    |
+----------------------------------------+------------------------------------------------+
| ``maxsize_zo``                         | Maximum number of integers in file name number |
+-----------------------------------------------------------------------------------------+

Note that even in backwards mode, the first and last number of the files to read are in the forwards direction, i.e. from 1 to your last file number. Of course this is not a comprehensive list of all the parameters you can set in the :file:`namelist`. More information can be found in the references listed at the start.


.. _Analyzing output:

Analyzing qualitative output
----------------------------

The NetCDF file that contains the particle tracks is named :file:`ariane_trajectories_qualitative.nc`. The variables in this file include the initial and final x, y, z, and time for the particles. It is a good idea to double check that these agree with the locations you listed in :file:`initial_positions.txt`. To plot and analyze the output, you read in traj_lon, traj_lat, traj_depth, and traj_time. These variables have the shape (number of particles, positions in time).

If you would like to see some examples of particle tracking, feel free to look at the following notebooks:

* `ParticleTracking.ipynb`_

.. _ParticleTracking.ipynb: https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/ParticleTracking.ipynb

Quantitative Mode
=================

This mode can be used to make estimates of transport through cross-sections by releasing a large number of particles and calculating how many particles pass through each section.

:file:`namelist`
----------------
The namelist for quantitative mode is very similar to qualitative mode, note the frequency of calculation is now set in the ``QUANTITATIVE`` section.
Here is an example of a quantitative namelist.

.. code-block:: fortran
   &ARIANE
       key_alltracers =.FALSE.,
       key_sequential =.FALSE.,
       key_ascii_outputs =.TRUE.,
       mode ='quantitative',
       forback ='forward',
       bin ='nobin',
       init_final ='init',
       nmax =30000,
       tunit =3600.,
       ntfic =1,
       key_computesigma=.FALSE.,
       zsigma=100.,
   /
   &OPAPARAM
       imt =398,
       jmt =898,
       kmt =40,
       lmt =24,
       key_periodic =.FALSE.,
       key_jfold =.FALSE.,
       key_computew =.FALSE.,
       key_partialsteps =.TRUE.,
   /
   &QUANTITATIVE
       key_eco        = .TRUE.,
       key_reducmem   = .TRUE.,
       key_unitm3     = .TRUE.,
       key_nointerpolstats = .FALSE.,
       max_transport  = 1.e4,
       lmin = 1,
       lmax = 6,
   /
   &ZONALCRT
       c_dir_zo ='/results/SalishSea/nowcast/01jan16/',
       c_prefix_zo ='SalishSea_1h_20160101_20160101_grid_U.nc',
       ind0_zo =-1,
       indn_zo =-1,
       maxsize_zo =-1,
       c_suffix_zo ='NONE',
       nc_var_zo ='vozocrtx',
       nc_var_eivu ='NONE',
       nc_att_mask_zo ='NONE',
   /
   &MERIDCRT
       c_dir_me ='/results/SalishSea/nowcast/01jan16/',
       c_prefix_me ='SalishSea_1h_20160101_20160101_grid_V.nc',
       ind0_me =-1,
       indn_me =-1,
       maxsize_me =-1,
       c_suffix_me ='NONE',
       nc_var_me ='vomecrty',
       nc_var_eivv ='NONE',
       nc_att_mask_me ='NONE',
   /
   &VERTICRT
       c_dir_ve     = '/results/SalishSea/nowcast/01jan16/',
       c_prefix_ve  = 'SalishSea_1h_20160101_20160101_grid_W.nc',
       ind0_ve      = -1,
       indn_ve      = -1,
       maxsize_ve   = -1,
       c_suffix_ve  = 'NONE',
       nc_var_ve    = 'vovecrtz',
       nc_var_eivw  = 'NONE',
       nc_att_mask_ve = 'NONE',
    /
    &TEMPERAT
       c_dir_te     = 'NONE',
       c_prefix_te  = 'NONE',
       ind0_te      = -1,
       indn_te      = -1,
       maxsize_te   = -1,
       c_suffix_te  = 'NONE',
       nc_var_te    = 'NONE',
       nc_att_mask_te = 'NONE',
    /
    &SALINITY
       c_dir_sa     = 'NONE',
       c_prefix_sa  = 'NONE',
       ind0_sa      = -1,
       indn_sa      = -1,
       maxsize_sa   = -1,
       c_suffix_sa  = 'NONE',
       nc_var_sa    = 'NONE',
       nc_att_mask_sa = 'NONE',
    /
    &MESH
       dir_mesh ='/data/nsoontie/MEOPAR/NEMO-forcing/grid/',
       fn_mesh ='mesh_mask_SalishSea2.nc',
       nc_var_xx_tt ='glamt',
       nc_var_xx_uu ='glamu',
       nc_var_yy_tt ='gphit',
       nc_var_yy_vv ='gphiv',
       nc_var_zz_ww ='gdepw',
       nc_var_e2u ='e2u',
       nc_var_e1v ='e1v',
       nc_var_e1t ='e1t',
       nc_var_e2t ='e2t',
       nc_var_e3t ='e3t',
       nc_var_tmask ='tmask',
       nc_mask_val =0.,
    /

Key differences in namelist from qualitative mode
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

There are some key differences between the namelists in quantitative and qualitative mode.
Pay special attention to the following options:

``ARIANE``:

* :kbd:`nmax`: The maximum number of particles. This parameter is typically much higher in quantitative mode. Tip: set it really high, the actual amount of particles seeded will be based on what you set for :kbd:`.max_transport.` and its no fun to have your run crash because the :kbd:`.nmax.` has been met. 

``QUANTITATIVE``:

* :kbd:`key_eco`: Setting to :kbd:`.TRUE.` reduces CPU time.
* :kbd:`key_reducmem`: Setting to :kbd:`.TRUE.` reduces memory by only reading model data over selected region.
* :kbd:`key_unitm3`: Setting to :kbd:`.TRUE.` prints transport calculation in m^3/s instead of Sverdrups.
* :kbd:`max_transport`: Maximum transport (in m^3/s) that should not be exceeded by the transport associated with each initial particle. A lower values means more initial particles and higher accuracy. Example values are 1e9 for one particle in one model cell and 1e4 for typical experiments.
* :kbd:`lmin`: First time step to generate particles.
* :kbd:`lmax`: Last time step to generate particles (will keep running until lmt is reached just with no new particles seeded).


Defining Sections
-----------------
You must define a closed region in your domain for transport calculations.
Ariane calculates the mass transport between an initial section in your region and the other sections.
Ariane provides a couple of useful tools for defining the sections.

* :kbd:`mkseg0`: This program reads your land-ocean mask and writes it as a text file. Run this program in the same directory as your namelist. You may need to add the ariane executables to your path.

.. code-block:: bash
    mkseg0
* :file:`segrid`: After you run :kbd:`mkseg0`, you should see a new file called :file:`segrid`. Edit this file with

.. code-block:: bash
   nedit segrid
* If you turn off text wrapping, you might see something like this:

.. figure:: images/segrid.png

Land points are :kbd:`#` and ocean points are :kbd:`-`.

* Add numbered cross-sections to this file. Be sure your sections are over ocean points and not land points. Ariane will initialize particles along the section labelled as :kbd:`1` and will calculate transport through all other sections. Your sections must make up a closed volume. Place the :kbd:`@` symbol somewhere within your closed subdomain.

* Run :kbd:`mkseg`

.. code-block:: bash
    mkseg
* This will print out whether you succeeded or not and let you know the extent of the sections you defined. If something went wrong in editing :file:`segrid` the message will let you know how, this usually entails accidentally deleting a land point, not closing your boundaries, or forgetting to place the :kbd:`@` symbol somewhere!
* Once you have that without errors, section definitions will be copied automatically into a file called :file:`sections.txt`. The section definitions should look something like this::

     1   250   313  -409  -409     1    40 "1section"
     2   264   312   386   386     1    40 "2section"
     3     1   398     1   898     0     0 "Surface"

You can rename :kbd:`"1section"` and :kbd:`"2section"` to something more intuitive if you desire. The :kbd:`"Surface"` section won't be added automatically, you should as it as above, with 398 replaced by the x-index extent of your model and 898 by the y-index extent.
    
Analyzing quantitative output
-----------------------------

A new file called :file:`stats.txt` contains statistics about the initial and final particles through each section and the transport calculations. It might look something like this::

 total transport (in m3/s):    230033.88767527405       ( x lmt =   5520813.3042065771      )
     max_transport (in m3/s)  :    1000000000.0000000
     # particles              :        20380

     initial state                #  20380
      stats. for:          x         y         z         a
             min:   -123.457    48.946     0.500     0.000
             max:   -123.134    49.063   226.275     0.000
            mean:   -123.347    48.986    74.893     0.000
       std. dev.:      0.062     0.022    61.722     0.000

     meanders        166079.1572 0
     1section        .0000 1
     2section        63952.7799 2
     Surface         .0000 3
               total 230033.8877
         except mnds 63954.7305
                lost 1.9506

     final state        meanders #  11094
     stats. ini:          x         y         z         a
            min:   -123.457    48.946     0.500     0.006
            max:   -123.134    49.063   226.275    16.858
           mean:   -123.343    48.987    91.665     0.606
      std. dev.:      0.055     0.020    61.438     1.010
     stats. fin:          x         y         z         a
            min:   -123.458    48.945     0.019     0.006
            max:   -123.132    49.064   238.621    16.858
           mean:   -123.329    48.992    91.483     0.606
      std. dev.:      0.052     0.019    62.670     1.010

     final state        2section #   9285
     stats. ini:          x         y         z         a
            min:   -123.457    48.946     0.500     0.191
            max:   -123.134    49.063   226.275    16.074
           mean:   -123.357    48.982    31.337     1.715
      std. dev.:      0.075     0.028    35.675     1.639
     stats. fin:          x         y         z         a
            min:   -123.317    48.883     0.058     0.191
            max:   -123.079    48.970   151.722    16.074
           mean:   -123.192    48.929    25.504     1.715
      std. dev.:      0.068     0.025    25.477     1.639

* :kbd:`lost` are the particles not intercepted by any section.
* :kbd:`meanders` are the particles that go back out the source section (section 1). Note that this is actually a bad translation from the original french Ariane was writen in. It would be more accurate to think of these particles as :kbd:`loop`.

Ariane will also produce netCDF files :file:`ariane_positions_quantitative.nc` and :file:`ariane_statistics_quantitative.nc` that can be used to plot the particle trajectories and statistics.

Time Considerations
-------------------

Particles initialized later in the simulation do not have as much time to cross one of the sections so it could be beneficial to impose a maximum age of each particle. This can be achieved by modifying :file:`mod_criter1.f90` in :kbd:`src/ariane` as follows:

.. code-block:: fortran
    !----------------------------------------!
    !- ADD AT THE END OF EACH LINE "!!ctr1" -!
    !----------------------------------------!
    !criter1=.FALSE.                 !! ctr1
    !
    !------------!
    !- Examples -!
    !------------!
    !
         criter1=(abs(hl-fl).ge. lmt-lmax)   !! ctr1

* :kbd:`lmt` and :kbd:`lmax` should be substituted by the values you set in the namelist.
* NOTE: You must remake and install ariane when making a change to any of the fortran files.

Defining and tracking water masses
----------------------------------

You can also impose a density and/or salinity and/or temperature criteria on the initial particles in order to track different water masses. You can achieve this by editing :file:`mod_criter0.f90`.


.. code-block:: fortran
    !criter0=.TRUE.        !!ctr0
    !
    !------------!
    !- Examples -!
    !------------!
         criter0=(zinter(ss,hi,hj,hk,hl).le.29_rprec)     !!crt0

* Once again, you must remake and install ariane.
* You'll also need to make sure that :kbd:`key_alltracers` and :kbd:`key_computesigma` are :kbd:`.TRUE.` and :kbd:`zsigma` are defined in your namelist.
* Now particles will only be initialized if they have a salinity less than 29.
* There are other examples of useful criteria in :file:`mod_criter0.f90`.
* NOTE: This same water mass tracking can also be done easily in your analysis of the output by filtering for parcticles that started with a salinity (:kbd:`data.init_salt`) less than 29. 

Using tracers in Ariane
=======================

Ariane can help us analyze tracers along the particle's trajectory (qualitative mode) or when the particle is entering leaving the analysis domain (quantitative mode).

The following items must be changed or added to the namelist file:

* :kbd:`key_alltracers`: :kbd:`.TRUE.` to print tracer information in diagnostics.
* :kbd:`key_computesigma`: :kbd:`.TRUE.` to compute density from temperature and salinity (set to :kbd:`.FALSE.` if the tracers you have input into the temperature and salinity fields are not actually temperature and salinity).
* :kbd:`zsigma`: Reference depth for calculation of density.
* ``TEMPERAT`` and ``SALINITY`` setions of the namelist file must be included as shown below, in order to tell Ariane where to look for the temperature and salinity model output

Temperature
^^^^^^^^^^^

 .. code-block:: fortran
    &TEMPERAT
	    c_dir_te ='/ocean/nsoontie/MEOPAR/SalishSea/results/storm-surges/final/dec2006/all_forcing/30min/',
	    c_prefix_te ='SalishSea_30m_20061214_20061215_grid_T.nc',
	    ind0_te =-1,
	    indn_te =-1,
	    maxsize_te =-1,
	    c_suffix_te ='NONE',
	    nc_var_te ='votemper',
	    nc_att_mask_te ='NONE',
    /


Salinity
^^^^^^^^

.. code-block:: fortran
    &SALINITY
	    c_dir_sa ='/ocean/nsoontie/MEOPAR/SalishSea/results/storm-surges/final/dec2006/all_forcing/30min/',
	    c_prefix_sa ='SalishSea_30m_20061214_20061215_grid_T.nc',
	    ind0_sa =-1,
	    indn_sa =-1,
	    maxsize_sa =-1,
	    c_suffix_sa ='NONE',
	    nc_var_sa ='vosaline',
	    nc_att_mask_sa ='NONE',
    /
:kbd:`votemper` and :kbd:`vosaline` are the variable names for temperature and salinity in our input file.

Output
------

In qualitative results (:file:`ariane_trajectories_qualitative.nc`) the variable names for the tracers are:

* traj_temp
* traj_salt
* traj_dens

In quantitative results (:file:`aariane_positions_quantitative.nc`) the variable names for the tracers are:

* init_salt
* init_temp
* init_dens
* final_salt
* final_temp
* final dens

A sample of Ariane qualitative tracer results can be found `here`_.

.. _here: https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_Tracers.ipynb


Multi-day runs
==============

Until now, we have entered only one input file into Ariane. Use Ariane's sequential mode to enter multiple files.

Input Files
-----------

The NetCDF files used as input must have the following format:
*prefix_number_suffix*

If the file names do not follow this format, create symbolic links that do. Create this link by using the command :kbd:`ln -s [target file directory] [symbolic link name]`

For example, you may consider:

* *prefix* = **SalishSea_**
* *number* = **01**, **02**, **etc**
* *suffix* = **_grid_T.nc**, **_grid_U.nc**, **_grid_V.nc**

Note: *number* must contain a constant digit number and its value must increase by one in chronological order. For example, file **SalishSea_01_grid_T.nc** contains tracers for November 1st and **SalishSea_02_grid_T.nc** contains tracers for November 2nd.



Namelist: Modify Sections
-------------------------

First, let's take a closer look at the parameters in the namelist sections. The parameter names are :kbd:`c_dir_X`, :kbd:`c_prefix_X`, :kbd:`ind0_X`, :kbd:`indn_X`, :kbd:`maxsize_X`, and :kbd:`c_suffix_X` where :kbd:`X` is :kbd:`zo`, :kbd:`me`, :kbd:`te`, :kbd:`sa` for the **ZONALCRT**, **MERIDCRT**, **TEMPERAT**, and **SALINITY** sections, respectively.


Input File Directory
^^^^^^^^^^^^^^^^^^^^
:kbd:`c_dir_X` is the directory with the symbolic links for the input files, the namelist, and the initial positions text file.


New Input Filename
^^^^^^^^^^^^^^^^^^
Previously, we have been entering the full filename, *SalishSea_t_yyyymmdd_yyyymmdd_grid_T.nc*, into :kbd:`c_prefix_X`.

Now that we have formatted the filenames as *prefix_number_suffix*, :kbd:`c_prefix_me` takes on the value of the *prefix* and :kbd:`c_suffix_me` takes the value of *suffix*.

:kbd:`ind0_X` is the *number* for the earliest input file and :kbd:`indn_X` is the latest.

:kbd:`maxsize_X` is the number of digits in *number*.

For example, the **ZONALCRT** section would look like the following for input files **SalishSea_01_grid_U.nc** and **SalishSea_02_grid_U.nc** :

 .. code-block:: fortran
        &ZONALCRT
        	c_dir_zo ='/ocean/imachuca/MEOPAR/Ariane/results/drifter_compare/sequential/',
        	c_prefix_zo ='SalishSea_',
	    	ind0_zo =01,
	    	indn_zo =02,
	    	maxsize_zo =2,
	    	c_suffix_zo ='_grid_U.nc',
	    	nc_var_zo ='vozocrtx',
	    	nc_var_eivu ='NONE',
	    	nc_att_mask_zo ='NONE',
        /


Sequential Parameter
^^^^^^^^^^^^^^^^^^^^
Under the **ARIANE** section in :kbd:`namelist`, change :kbd:`key_sequential` to TRUE.



Namelist: Add Section
---------------------

Add a **SEQUENTIAL** section in namelist. This section has one parameter, :kbd:`maxcycles`. We recommend the value of this parameter to be 1 since this tells Ariane to stop generating trajectory points once it has run out of input data.


Sequential
^^^^^^^^^^

 .. code-block:: fortran
	&SEQUENTIAL
	maxcycles =1,
	/



Results
-------
The results produced for the example above:

* `Ariane_Sequential.ipynb`_

.. _Ariane_Sequential.ipynb: https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_Sequential.ipynb


Frequency Sensitivity Sample
============================

The model produces datasets containing information about the velocity field for a region. Ariane uses this information to produce particle trajectories. We wanted to know at what frequency would the model output need to be to produce the most reliable particle trajectories.

For the frequency sensitivity studies, we used model outputs with 30 minute, 1 hour, and 4 hour frequencies. This data was used in Ariane's qualitative mode to generate particle trajectories with points at 30 minute intervals. We did this for particles starting their trajectories at the Fraser River and at various points along the thalweg. The results are summarized below, the sansitivity analysis that led to these results can be found `here`_.

.. _here: https://nbviewer.org/github/SalishSeaCast/analysis/blob/master/Idalia/Ariane_TimeRes.ipynb

On the Surface
===================

At the Fraser River, we found that the particle trajectory generated using data at a 4 hour frequency does not capture subtleties in particle motion as do the trajectories derived from data at 30 minute and 1 hour frequencies.
The trajectory that used 1 hour frequency data very closely resembles the trajectory that used 30 minute data.

:command:`Conclusion: We can use 1 hour or 30 minute NEMO output data when particle trajectories start at the Fraser River.`

At Depth
===================
:command:`Conclusion: In regions of moderate mixing, 30 minute data would be preferable; we can use 1 hour data with some caution. In regions of heavy mixing, we should exercise caution in analyzing trajectories and depths since results vary greatly depending on frequencies.`


