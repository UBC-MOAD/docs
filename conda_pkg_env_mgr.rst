.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MOAD-CondaPkgAndEnvMgr:

*************************************
Conda Package and Environment Manager
*************************************

We use `Conda`_ software packages,
dependencies,
and environments.
:program:`conda` was developed as part of the excellent `Anaconda data science toolkit`_,
but we have found that using :program:`conda` and explicitly managing software environments
is more robust over time than using Anaconda.

.. _Conda: https://docs.conda.io/en/latest/
.. _Anaconda data science toolkit: https://www.anaconda.com/download


TODO: Write an intro to conda

Slides notebook from May 2023 group discussion of Python packages and environments:

* nbviewer: https://nbviewer.org/github/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb
* GitHub: https://github.com/UBC-MOAD/PythonNotes/blob/main/pkgs-envs/PythonPkgsEnvsSlides-2023.ipynb


.. _InstallingMiniforge:

Installing Miniforge
====================

`Miniforge`_ is a minimal installer for `Conda`_ specific to `conda-forge`_.
It installs the :program:`conda` package and environment manager tool,
a recent version of Python, the packages that :program:`conda` depends on,
and a small number of other packages that are essential for creating and managing software environments.

.. _Miniforge: https://github.com/conda-forge/miniforge
.. _conda-forge: https://conda-forge.org/

The installers for Miniforge are linked on the `Miniforge`_ page.
You can download them from there,
and that is what you should do if you are working on Windows.
But for Linux and MacOS you'll be working with :program:`conda` on the command-line in terminals sessions,
so you might as well get started that way by using :program:`wget` to download the installer script into your home directory.

On Linux use:

.. code-block:: bash

    $ wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh

On MacOS use:

.. code-block:: bash

    $ wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-MacOSX-x86_64.sh

The installation instructions are in the "Unix-like platforms" section of the README of
https://github.com/conda-forge/miniforge.
In short:

#. Run the installer script via :program:`bash`:

   On Linux use:

   .. code-block:: bash

      $ bash Miniforge3-Linux-x86_64.sh

   On MacOS use:

   .. code-block:: bash

      $ bash Miniforge3-MacOSX-x86_64.sh

   Follow the prompts on the screen.
   Accept the defaults offered for all of the settings

#. To make the changes take effect,
   close and then re-open your terminal window.

#. Test your installation. In your terminal window,
   run the command :command:`conda list`.
   A list of installed packages appears if it has been installed correctly;
   it looks something like:

   .. code-block:: text

        # packages in environment at /home/dlatorne/miniconda3:
        #
        # Name                    Version                   Build  Channel
        _libgcc_mutex             0.1                 conda_forge    conda-forge
        _openmp_mutex             4.5                       1_gnu    conda-forge
        brotlipy                  0.7.0           py37h6b43d8f_1003    conda-forge
        bzip2                     1.0.8                h7f98852_4    conda-forge
        c-ares                    1.18.1               h7f98852_0    conda-forge
        ca-certificates           2021.10.8            ha878542_0    conda-forge
        certifi                   2021.10.8        py37h9c2f6ca_1    conda-forge
        cffi                      1.14.6                 0_pypy37    conda-forge
        charset-normalizer        2.0.8              pyhd8ed1ab_0    conda-forge
        colorama                  0.4.4              pyh9f0ad1d_0    conda-forge
        conda                     4.10.3           py37h9c2f6ca_4    conda-forge
        conda-package-handling    1.7.3            py37h6b43d8f_1    conda-forge
        cryptography              36.0.0           py37h5c3f282_0    conda-forge
        expat                     2.4.1                h9c3ff4c_0    conda-forge
        gdbm                      1.18                 h0a1914f_2    conda-forge
        icu                       69.1                 h9c3ff4c_0    conda-forge
        idna                      3.1                pyhd3deb0d_0    conda-forge
        krb5                      1.19.2               hcc1bbae_3    conda-forge
        libarchive                3.5.2                hccf745f_1    conda-forge
        libcurl                   7.80.0               h2574ce0_0    conda-forge
        libedit                   3.1.20191231         he28a2e2_2    conda-forge
        libev                     4.33                 h516909a_1    conda-forge
        libffi                    3.4.2                h7f98852_5    conda-forge
        libgcc-ng                 11.2.0              h1d223b6_11    conda-forge
        libgomp                   11.2.0              h1d223b6_11    conda-forge
        libiconv                  1.16                 h516909a_0    conda-forge
        libmamba                  0.19.0               h3985d26_0    conda-forge
        libmambapy                0.19.0           py37h9bd18e5_0    conda-forge
        libnghttp2                1.43.0               h812cca2_1    conda-forge
        libsolv                   0.7.19               h780b84a_5    conda-forge
        libssh2                   1.10.0               ha56f1ee_2    conda-forge
        libstdcxx-ng              11.2.0              he4da1e4_11    conda-forge
        libxml2                   2.9.12               h885dcf4_1    conda-forge
        libzlib                   1.2.11            h36c2ea0_1013    conda-forge
        lz4-c                     1.9.3                h9c3ff4c_1    conda-forge
        lzo                       2.10              h516909a_1000    conda-forge
        mamba                     0.19.0           py37h47bf687_0    conda-forge
        ncurses                   6.2                  h58526e2_4    conda-forge
        openssl                   1.1.1l               h7f98852_0    conda-forge
        pip                       21.3.1             pyhd8ed1ab_0    conda-forge
        pybind11-abi              4                    hd8ed1ab_3    conda-forge
        pycosat                   0.6.3           py37h6b43d8f_1009    conda-forge
        pyopenssl                 21.0.0             pyhd8ed1ab_0    conda-forge
        pypy3.7                   7.3.7                hbc09475_3    conda-forge
        pysocks                   1.7.1            py37h9c2f6ca_4    conda-forge
        python                    3.7.12                0_73_pypy    conda-forge
        python_abi                3.7               2_pypy37_pp73    conda-forge
        readline                  8.1                  h46c0cb4_0    conda-forge
        reproc                    14.2.3               h7f98852_0    conda-forge
        reproc-cpp                14.2.3               h9c3ff4c_0    conda-forge
        requests                  2.26.0             pyhd8ed1ab_1    conda-forge
        ruamel_yaml               0.15.80         py37h6b43d8f_1006    conda-forge
        setuptools                59.4.0           py37h9c2f6ca_0    conda-forge
        six                       1.16.0             pyh6c4a22f_0    conda-forge
        sqlite                    3.37.0               h9cd32fc_0    conda-forge
        tk                        8.6.11               h27826a3_1    conda-forge
        tqdm                      4.62.3             pyhd8ed1ab_0    conda-forge
        urllib3                   1.26.7             pyhd8ed1ab_0    conda-forge
        wheel                     0.37.0             pyhd8ed1ab_1    conda-forge
        xz                        5.2.5                h516909a_1    conda-forge
        yaml                      0.2.5                h516909a_0    conda-forge
        yaml-cpp                  0.6.3                he1b5a44_4    conda-forge
        zlib                      1.2.11            h36c2ea0_1013    conda-forge
        zstd                      1.5.0                ha95c52a_0    conda-forge


.. _condaConfiguration:

:program:`conda` Configuration
==============================

:program:`conda` uses configuration settings in your :file:`$HOME/.condarc` file to supplement its default configuration.
You need to set up this configuration on each machine that you use :program:`conda` on;
i.e. on your laptop,
and on the Waterhole workstation that you use
(which will cover all of the Waterhole/Ocean machines).

The :command:`conda config` command is how you interact with the :file:`$HOME/.condarc` file.
Start by telling :program:`conda` where you want to store your environments:

.. code-block:: bash

    $ conda config --prepend envs_dirs $HOME/conda_envs/
    $ mkdir $HOME/conda_envs/

The first of those lines tells :program:`conda` that you want to put your environments
in a directory called :file:`$HOME/conda_envs/`.
The second line creates that directory.
Storing environment directory trees outside of the :file:`$HOME/miniforge3/` directory
created by the installer means that if you need to re-install Miniforge
you can do so without destroying all of your environments.

If you want to see all of the :program:`conda` configuration settings
(both the defaults,
and the supplements from your :file:`$HOME/.condarc` file,
you can use:

.. code-block:: bash

    $ conda config --show

There are many,
many things that you can configure in :program:`conda`.
If you want to see all of the gory details,
please see the `conda config docs`_.

.. _conda config docs: https://conda.io/projects/conda/en/latest/user-guide/configuration/use-condarc.html
