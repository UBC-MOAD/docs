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
  Field definition elements may
  (and generally should)
  also contain metadata attributes such as long name,
  standard name,
  and units.
  Please see the :ref:`field_def.xmlFile` section below for more information about the structure and contents of :file:`field_def.xml` files.

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


.. _CustomizingXML-Files:

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


.. _field_def.xmlFile:

:file:`field_def.xml`
---------------------

This section provides some information about the structure and contents of a :file:`field_def.xml` file.
This is *not* an exhaustive reference guide for all of the possible attribute values;
for that,
please see chapter 3 of the `XIOS User Guide`_.

.. _XIOS User Guide: http://forge.ipsl.jussieu.fr/ioserver/raw-attachment/wiki/WikiStart/XIOS_user_guide.pdf

:file:`NEMO-3.6-code/NEMOGCM/CONFIG/SHARED/field_def.xml` is the reference version of the file that is provided with the NEMO code.
In many cases,
you can use that reference file by putting its path as the value of the :kbd:`filedefs` element in the :kbd:`output` section of your run description YAML file
(see :ref:`CommandProcessorsAndXML-Files`).
Reasons why you might want to create your own customized version
(see :ref:`CustomizingXML-Files`)
of :file:`field_def.xml` include:

* Adding new variable(s) to NEMO that you want to include in your output files
* Adjusting/correcting the values of variable field attributes such as :kbd:`long_name`,
  :kbd:`standard_name`,
  :kbd:`unit`,
  etc.
  Those attributes provide variable-level metadata items in output files.

Here is an example fragment of a :file:`field_def.xml` file:

.. code-block:: xml

   <field_definition level="1" prec="4" operation="average" enabled=".TRUE." default_value="1.e20">
    <field_group id="grid_T" grid_ref="grid_T_2D" >
      <field id="sst" long_name="sea surface temperature" standard_name="sea_surface_temperature" unit="degC"/>
      <field id="toce" long_name="temperature" standard_name="sea_water_conservative_temperature" unit="degC" grid_ref="grid_T_3D"/>

      <field id="sss" long_name="sea surface salinity" standard_name="sea_surface_reference_salinity" unit="g kg-1"/>
      <field id="soce" long_name="salinity" standard_name="sea_water_reference_salinity" unit="g kg-1" grid_ref="grid_T_3D"/>

      <field id="sst2" long_name="square of sea surface temperature" standard_name="square_of_sea_surface_temperature" unit="degC2">
        sst * sst
      </field >

      <field id="sstmax" long_name="max of sea surface temperature" field_ref="sst" operation="maximum"/>
      ...
    </field_group>
    ...
   </field_definition>

:file:`field_def.xml` files contain 3 types of tags:

* :kbd:`field_definition`
* :kbd:`field_group`
* :kbd:`field`

:kbd:`field` tags must be contained within a :kbd:`field_group` tag,
which must me contained within a :kbd:`field_definition` tag.

Attributes included in a tag apply to all contained tags unless they are explicitly overridden in a contained tag.
So the :kbd:`operation="average"` attribute in:

.. code-block:: xml

   <field_definition level="1" prec="4" operation="average" enabled=".TRUE." default_value="1.e20">

means that all field values will be averaged over the output time interval unless a different :kbd:`operation` is specified in the :kbd:`field` tag,
for example:

.. code-block:: xml

      <field id="sstmax" long_name="max of sea surface temperature" field_ref="sst" operation="maximum"/>

in which case the maximum value over the output time interval of the :kbd:`sst` field
(specified by the :kbd:`field_ref` attribute)
will be calculated by XIOS.

The :kbd:`operation` attribute enables the burden of calculating various temporal quantities on field variables to be shifted from NEMO to XIOS.
Please see section 3.2 of the `XIOS User Guide`_ for details.

Another way of doing field operations in XIOS is to specify them in the :kbd:`field` tag,
for example:

.. code-block:: xml

    <field id="sst2" long_name="square of sea surface temperature" standard_name="square_of_sea_surface_temperature" unit="degC2">
      sst * sst
    </field >

Here again,
the burden of declaration,
memory allocation,
and calculation of the :kbd:`sst2` variable is shifted from NEMO to XIOS.
This form of field calculation can be useful for calculating fluxes.

:kbd:`field_group` tags specify the default grid on which the contained :kbd:`field` tags are defined via the :kbd:`grid_ref` attribute.
That attribute can,
of course,
be overridden in the contained :kbd:`field` tags.

All :kbd:`field` tags should have the following attributes:

* :kbd:`long_name`
* :kbd:`standard_name`
* :kbd:`unit`

Those attributes are passed through to the netCDF output files as field variable metadata.

Values for the :kbd:`standard_name` attribute should be chosen from the `CF conventions standard names table`_.
Standard names are written in "snake case"
(words separated by :kbd:`_` characters).
That table also provides canonical units that should be used at the value of the :kbd:`unit` attribute.

.. _CF conventions standard names table: http://cfconventions.org/Data/cf-standard-names/29/build/cf-standard-name-table.html

The value of the :kbd:`long_name` attribute can be more free-from and descriptive. It is typically used for plot axis labels,
table headings,
etc.

In addition to :file:`NEMO-3.6-code/NEMOGCM/CONFIG/SHARED/field_def.xml`,
there are examples of :file:`field_def.xml` files in the `SS-run-sets/v201702/`_ directory tree.

.. _SS-run-sets/v201702/: https://bitbucket.org/salishsea/ss-run-sets/src/tip/v201702/


.. _SwitchingFromXIOS-1toXIOS-2:

Switching from XIOS-1 to XIOS-2
===============================

The main changes when switching from XIOS-1 to XIOS-2 are to the XML configuration files. These changes are described in the sections below. In addition, you will need to add "key_xios2" to your list of cpp keys in your NEMO configuration, and if you are using NEMO-cmd, you will need to link the location of your :file:`file_def.xml` and XIOS-2 folder in your :file:`config.yaml`.

Changes to iodef.xml
--------------------

First, remove the file definition section from :file:`iodef.xml` and move it to a new file named :file:`file_def.xml` (see the following section for more information). The file definition will now be loaded similar to :file:`domain_def.xml` and :file:`field_def.xml`. To do this, add the following lines to :file:`iodef.xml`:

.. code-block:: XML

    <file_definition src="./file_def.xml"/>

The formatting of the grids within the grid definition section will also need to be changed. As an example, in XIOS-1 grid_T is defined as:

.. code-block:: XML

    <grid id="grid_T_2D" domain_ref="grid_T"/>
    <grid id="grid_T_3D" domain_ref="grid_T" axis_ref="deptht"/>

While, in XIOS-2 it becomes:

.. code-block:: XML

    <grid id="grid_T_2D"> <domain domain_ref="grid_T"> </domain> </grid>
    <grid id="grid_T_3D"> <domain domain_ref="grid_T"> </domain> <axis id="deptht"> </axis> </grid>

Another difference is that XIOS-2 calculates buffersize, compared to XIOS-1 where it is user-specified. The following lines are changed/added in XIOS-2 to specify variables to do with the buffersize:

.. code-block:: XML

  <context id="xios">
    <variable_definition>
      <variable id="optimal_buffer_size"       type="string">performance</variable>
      <variable id="buffer_size_factor"        type="double">1.0</variable>
      <variable id="info_level"                type="int" >10</variable>
    </variable_definition>
  </context>


Create file_def.xml
-------------------

The content of the file_definition section of :file:`iodef.xml` in XIOS-1 is moved to a seperate file: :file:`file_def.xml` in XIOS-2. In addition, the file definition needs to be changed from:

.. code-block:: XML

   <file_definition type="multiple_files" name="@expname@_@freq@_@startdate@_@enddate@" sync_freq="1d" min_digits="4">

to:

.. code-block:: XML

   <file_definition type="one_file" name="@expname@_@freq@_@startdate@_@enddate@" sync_freq="1d" min_digits="4">

For each file group, you will want to specify a compression level:

.. code-block:: XML

   <file_group id="1ts" output_freq="1ts" output_level="10" compression_level="4" enabled=".TRUE."> </file_group>


Changes to domain_def.xml
-------------------------

The only changes to :file:`domain_def.xml` occur in the domain statements which need to be reformatted for XIOS-2. For example, for grid_T in XIOS-1 we had:

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


