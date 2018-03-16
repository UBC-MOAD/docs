.. Copyright 2018 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   http://creativecommons.org/licenses/by/4.0/


.. XIOS-2-docs:

******
XIOS-2
******

`XIOS`_ is the IO management library that is used by NEMO to produce netCDF results files.
NEMO-3.6 uses XIOS-2.

.. _XIOS: http://forge.ipsl.jussieu.fr/ioserver/wiki

XIOS operates as a server process that multiple NEMO calculation processes communicate with to write results to netCDF files.
XIOS buffers the output from NEMO in memory while the much slower process of writing to disk happens.
That means that the NEMO processes can do calculations almost continuously without having to periodically pause to write results to disk.

In many cases,
especially on the ComputeCanada HPC clusters with large memory per node,
it is possible to configure NEMO runs with tens to a hundred or more processors to use a single XIOS process.
The XIOS process must be started in the same :command:`mpirun` command as NEMO.
Fortunately,
the :ref:`nemocmd:NEMO-CommandProcessor` and :ref:`salishseacmd:SalishSeaCmdProcessor` tools take care of doing that for you.


.. _GettingXIOS-2:

Getting XIOS-2
==============

The MOAD group maintains our own Mercurial repositories on Bitbucket of the `XIOS-2 code`_ and `build configuration files`_ for building XIOS-2 on the compute platforms that we use.
Our XIOS-2 code repo is accessible only by members of the MOAD group so as to respect the sign-up requirement of the upstream `XIOS`_ repository.
The architecture file repo is public so that other researchers can make use of the build system options that we have figured out for various system.

.. _XIOS-2 code: https://bitbucket.org/salishsea/xios-2
.. _build configuration files: https://bitbucket.org/salishsea/xios-arch

To get the XIOS-2 code and build configuration files repos you need to clone them into you research project directory
(typically one of :file:`MEOPAR/`,
:file:`GEOTRACES/`,
or :file:`CANYONS`).
Here are some examples of commands to do that on various platforms that we use.
*You should substitute your own research project directory name as appropriate.*


:kbd:`cedar` or :kbd:`graham`
-----------------------------

.. code-block:: bash

    cd $PROJECT/$USER/GEOTRACES
    hg clone ssh://hg@bitbucket.org/salishsea/xios-2 XIOS-2
    hg clone ssh://hg@bitbucket.org/salishsea/xios-arch XIOS-ARCH


:kbd:`orcinus`
--------------

.. code-block:: bash

    cd $HOME/MEOPAR
    hg clone ssh://hg@bitbucket.org/salishsea/xios-2 XIOS-2
    hg clone ssh://hg@bitbucket.org/salishsea/xios-arch XIOS-ARCH


:kbd:`salish`
-------------

.. code-block:: bash

    cd /data/$USER/CANYONS
    hg clone ssh://hg@bitbucket.org/salishsea/xios-2 XIOS-2
    hg clone ssh://hg@bitbucket.org/salishsea/xios-arch XIOS-ARCH


.. _BuildingXIOS-2:

Building XIOS-2
===============

First symlink the XIOS-2 build configuration files for the machine that you are working on from the :file:`XIOS-ARCH` repo clone into the :file:`XIOS-2/arch/` directory,
then compile and link XIOS-2. *You should substitute your own research project directory name as appropriate.* Define the destination of your XIOS home directory as an environment variable by adding the following line to your :file:`.bashrc`:

.. code-block:: bash

    export XIOS_HOME=$PROJECT/$USER/GEOTRACES/XIOS-2/

:kbd:`cedar`:
-------------

.. code-block:: bash

    cd $PROJECT/$USER/MEOPAR/XIOS-2/arch
    ln -sf $PROJECT/$USER/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_CEDAR.env
    ln -sf $PROJECT/$USER/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_CEDAR.fcm
    ln -sf $PROJECT/$USER/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_CEDAR.path
    cd $PROJECT/$USER/MEOPAR/XIOS-2
    ./make_xios --arch X64_CEDAR --job 8


:kbd:`graham`:
--------------

.. code-block:: bash

    cd $PROJECT/$USER/GEOTRACES/XIOS-2/arch
    ln -sf $PROJECT/$USER/GEOTRACES/XIOS-ARCH/WESTGRID/arch-X64_GRAHAM.env
    ln -sf $PROJECT/$USER/GEOTRACES/XIOS-ARCH/WESTGRID/arch-X64_GRAHAM.fcm
    ln -sf $PROJECT/$USER/GEOTRACES/XIOS-ARCH/WESTGRID/arch-X64_GRAHAM.path
    cd $PROJECT/$USER/GEOTRACES/XIOS-2
    ./make_xios --arch X64_GRAHAM --job 8


:kbd:`orcinus`
--------------

.. code-block:: bash

    cd $HOME/MEOPAR/XIOS-2/arch
    ln -sf $HOME/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_ORCINUS.env
    ln -sf $HOME/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_ORCINUS.fcm
    ln -sf $HOME/MEOPAR/XIOS-ARCH/WESTGRID/arch-X64_ORCINUS.path
    cd $HOME/MEOPAR/XIOS-2
    ./make_xios --arch X64_ORCINUS --netcdf_lib netcdf4_par --job 8


:kbd:`salish`
-------------

.. code-block:: bash

    cd /data/$USER/CANYONS/XIOS-2/arch
    ln -sf /data/$USER/CANYONS/XIOS-ARCH/UBC-EOAS/arch-GCC_SALISH.fcm
    ln -sf /data/$USER/CANYONS/XIOS-ARCH/UBC-EOAS/arch-GCC_SALISH.path
    cd /data/$USER/CANYONS/XIOS-2
    ./make_xios --arch GCC_SALISH --netcdf_lib netcdf4_seq --job 8


.. _XIOS-2CleanBuild:

Doing a Clean Build
-------------------

If you need to do a clean build of XIOS-2,
you can use:

.. code-block:: bash

    cd $PROJECT/$USER/MEOPAR/XIOS-2
    ./tools/FCM/bin/fcm build --clean
    ./make_xios --arch X64_CEDAR --job 8

to clear away all artifacts of the previous build.

*Be sure to substitute the appropriate research project directory name and machine name.*


.. _XIOS-2ConfigurationFiles:

XIOS-2 Configuration Files
==========================

To use XIOS-2 with NEMO,
four configuration files written in `XML`_ are required:

.. _XML: https://en.wikipedia.org/wiki/XML

* :file:`field_def.xml` defines the variables that can be output and the grids on which they are defined.
  Field definition element may
  (and generally should)
  also contain metadata attributes such as long name,
  standard name,
  and units.

* :file:`domain_def.xml` defines "zoomed" sub-domains of the model domain and the grids on which they are defined.
  The "zooms" are defined on the i-j (x-y) directions,
  regardless of the depth of the sub-domain.

* :file:`iodef.xml` defines the vertical extent of output grids in the :kbd:`axis` elements,
  and the output grids.
  It also contains a separate :kbd:`context` element for :kbd:`xios` in which a few settings that control XIOS-2 are declared.

* :file:`file_def.xml` defines the files into which field variables are output and the frequency of output of those files.
  Variable names can be transformed from the internal NEMO names to more user friendly names in the :kbd:`field` elements in this file.
  This is also where on-the-fly deflation of output files is enabled via the :kbd:`compression_level="4"` attribute of :kbd:`file_group` elements.

.. warning::
    XML syntax is very exacting,
    so care is required when you edit XML files to ensure that tags are correctly closed,
    attribute values are correctly quoted,
    etc.

    Annoyingly,
    NEMO will fail *with no diagnostic messages* if your XML files contain errors.
    If you suspect that you have made an error in editing an XML file,
    one way of checking is to use an online validator like https://www.xmlvalidation.com/.


Customizing XML Files
---------------------

The `NEMO-3.6-code`_ repositories contains sample XIOS-2 configuration files in the :file:`NEMOGCM/CONFIG/SHARED/` and some of the :file:`NEMOGCM/CONFIG/*/EXP00/` directories.
*Please* **do not** *modify and commit those files.*
Doing so will cause conflicts when changes to NEMO are pulled in from the upstream repository,
and your changes will be overwritten.
Instead,
put copies of the XML files that you want to change under version control in your runs configuration repo
(for example, the `SS-run-sets`_ repo for people working on MEOPAR).

.. _NEMO-3.6-code: https://bitbucket.org/salishsea/nemo-3.6-code
.. _SS-run-sets: https://bitbucket.org/salishsea/ss-run-sets


.. _CommandProcessorsAndXML-Files:

Command Processors and XML Files
--------------------------------

The :ref:`nemocmd:NEMO-CommandProcessor` and :ref:`salishseacmd:SalishSeaCmdProcessor` tools provide a way,
via YAML run description files,
to map XML files with arbitrary file names and directory paths on to the file names that NEMO requires in the directory from which NEMO is executed.

The :kbd:`output` section of the YAML description file is where the XML file mappings and other XIOS-2 settings are specified.
Please see the `salishsea YAML file output section`_ docs if you are working on the Salish Sea configurations of NEMO,
or the `nemo YAML file output section`_ docs if you use another NEMO configuration.
There are also examples of complete YAML run description files in those docs.

.. _salishsea YAML file output section: https://salishseacmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section
.. _nemo YAML file output section: https://nemo-cmd.readthedocs.io/en/latest/run_description_file/3.6_yaml_file.html#output-section

The simplest possible YAML file :kbd:`output` section is:

.. code-block:: yaml

    output:
      iodefs: iodef.xml
      filedefs: file_def.xml
      domaindefs: domain_def.xml
      fielddefs: field_def.xml
      separate XIOS server: True
      XIOS servers: 1

In this case,
the XML files are all in the same directory as the YAML file.
If you use relative paths,
they have to be relative to the directory where the YAML file is.

A more complicated example is:

.. code-block:: yaml

    output:
      separate XIOS server: True
      XIOS servers: 1
      iodefs: iodef.xml
      filedefs: $HOME/CANYONS/mackenzie_canyon/output/file_def_realistic.xml
      domaindefs: ../domain_def.xml
      fielddefs: $HOME/CANYONS/mackenzie_canyon/output/field_def.xml

Note the use of:

* A relative path for :kbd:`domaindefs`
* Absolute paths containing the environment variable :envvar:`$HOME` for :kbd:`filedefs` and :kbd:`fielddefs`.
  Other environment variables like :envvar:`$USER`,
  :envvar:`$PROJECT`,
  and :envvar:`$SCRATCH` can also be used in XML file paths.
* The more descriptive file name :file:`file_def_realistic.xml` for :kbd:`filedefs`

.. _SwitchingFromXIOS-1toXIOS-2:
Switching from XIOS-1 to XIOS-2:
================================

The main changes that need to be made when switching from XIOS-1 to XIOS-2 are to the XML configuration files.

Make changes to iodef.xml:
--------------------------------
First, remove the file definition section from :file:`iodef.xml` and move it to a new file named :file:`file_def.xml` (see the following section for more information). The file definition will now be loaded similar to :file:`domain_def.xml` and :file:`field_def.xml`. To do this, add the following lines to :file:`iodef.xml`:

.. code-block:: XML

    -->
    <file_definition src="./file_def.xml"/>
    <!--

The formatting of the grid definition section is also changed. In XIOS-1 the grid is defined as:

.. code-block:: XML
    
    <grid id="grid_T_2D" domain_ref="grid_T"/> 
    <grid id="grid_T_3D" domain_ref="grid_T" axis_ref="deptht"/>

While, in XIOS-2 it becomes:

.. code-block:: XML

    <grid id="grid_T_2D"> <domain domain_ref="grid_T"> </domain> </grid>
    <grid id="grid_T_3D"> <domain domain_ref="grid_T"> </domain> <axis id="deptht"> </axis> </grid>

Another difference is that XIOS-2 calculates buffersize, compared to XIOS-1 where it is user-specified. The following lines are changed/added in the context XIOS section in XIOS-2 to specify variables to do with the buffersize:

.. code-block:: XML

  <context id="xios">
    <variable_definition>
      <variable id="optimal_buffer_size"       type="string">performance</variable>
      <variable id="buffer_size_factor"        type="double">1.0</variable>
      <variable id="info_level"                type="int" >10</variable>
    </variable_definition>
  </context>

Create file_def.xml:
--------------------------------

The content of the file_definition section of :file:`iodef.xml` in XIOS-1 is moved to a seperate file: :file:`file_def.xml`, in XIOS-2. At the top of the file definition, the file definition needs to be changed from:

.. code-block:: XML

   <file_definition type="multiple_files" name="@expname@_@freq@_@startdate@_@enddate@" sync_freq="1d" min_digits="4">
To:

.. code-block:: XML

   <file_definition type="one_file" name="@expname@_@freq@_@startdate@_@enddate@" sync_freq="1d" min_digits="4">

Additionally, you will want to specify the compression level for each file group:

.. code-block:: XML 

   <file_group id="1ts" output_freq="1ts" output_level="10" compression_level="4" enabled=".TRUE."> </file_group>

Make changes to domain_def.xml:
--------------------------------

:file:`domain_def.xml` requires reformatting of the domain statements between XIOS-1 and XIOS-2. In XIOS-1 we had:

.. code-block:: XML
        <domain_group id="grid_T">
                <domain id="grid_T" long_name="grid T"/>
                <domain id="test_T" domain_ref="grid_T"/>
        </domain_group>
In XIOS-2 this becomes:

.. code-block:: XML
        <domain_group id="grid_T">
                <domain id="grid_T" long_name="grid T"/>
                <domain id="test_T" domain_ref="grid_T"> </domain>
        </domain_group>


