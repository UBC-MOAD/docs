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

We use `conda`_ software packages,
dependencies,
and environments.
:program:`conda` is part of the excellent `Anaconda data science toolkit`_,
but we have found that using :program:`conda` and explicitly managing software environments is more robust over time than using Anaconda.

.. _conda: https://docs.conda.io/en/latest/
.. _Anaconda data science toolkit: https://www.anaconda.com/products/individual


TODO: Write an intro to conda


.. _InstallingMiniconda:

Installing Miniconda
====================

.. important:: 
    *Do not* install Miniconda or Anaconda on the Compute Canada HPC clusters or other HPC systems!
    It will almost certainly conflict with the HPC-optimized software environments on those system.
    If you need to install Python packages on an HPC system,
    please contact Doug for advice.

`Miniconda`_ is a minimal installer for conda.
It installs the :program:`conda` package and environment manager tool,
a recent version of Python, the packages that :program:`conda` depends on, 
and a small number of other packages that are essential for creating and managing software environments.

.. _Miniconda: https://docs.conda.io/en/latest/miniconda.html

The installers for Miniconda are linked on the `Miniconda`_ page.
You can download them from there,
and that is what you should do if you are working on Windows.
But for Linux and MacOS you'll be working with :program:`conda` on the command-line in terminals sessions,
so you might as well get started that way by using :program:`wget` to download the installer script into your home directory.
On Linux use:

.. code-block:: bash

    $ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh

On MacOS use:

.. code-block:: bash

    $ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh

The installation instructions are linked from https://conda.io/projects/conda/en/latest/user-guide/install/index.html#regular-installation.
In short:

#. Run the installer script via :program:`bash`:

   On Linux use:

   .. code-block:: bash

      $ bash Miniconda3-latest-Linux-x86_64.sh

   On MacOS use:

   .. code-block:: bash

      $ bash Miniconda3-latest-MacOSX-x86_64.sh

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
        _libgcc_mutex             0.1                        main  
        brotlipy                  0.7.0           py39h27cfd23_1003  
        ca-certificates           2021.7.5             h06a4308_1  
        certifi                   2021.5.30        py39h06a4308_0  
        cffi                      1.14.6           py39h400218f_0  
        chardet                   4.0.0           py39h06a4308_1003  
        conda                     4.10.3           py39h06a4308_0  
        conda-package-handling    1.7.3            py39h27cfd23_1  
        cryptography              3.4.7            py39hd23ed53_0  
        idna                      2.10               pyhd3eb1b0_0  
        ld_impl_linux-64          2.35.1               h7274673_9  
        libedit                   3.1.20210216         h27cfd23_1  
        libffi                    3.3                  he6710b0_2  
        libgcc-ng                 9.1.0                hdf63c60_0  
        libstdcxx-ng              9.1.0                hdf63c60_0  
        ncurses                   6.2                  he6710b0_1  
        openssl                   1.1.1l               h7f8727e_0  
        pip                       21.2.4           py37h06a4308_0  
        pycosat                   0.6.3            py39h27cfd23_0  
        pycparser                 2.20                       py_2  
        pyopenssl                 20.0.1             pyhd3eb1b0_1  
        pysocks                   1.7.1            py39h06a4308_0  
        python                    3.9.6                h12debd9_1  
        readline                  8.1                  h27cfd23_0  
        requests                  2.25.1             pyhd3eb1b0_0  
        ruamel_yaml               0.15.100         py39h27cfd23_0  
        setuptools                52.0.0           py39h06a4308_0  
        six                       1.16.0             pyhd3eb1b0_0  
        sqlite                    3.36.0               hc218d9a_0  
        tk                        8.6.10               hbc83047_0  
        tqdm                      4.59.0             pyhd3eb1b0_1  
        tzdata                    2021a                h5d7bf9c_0  
        urllib3                   1.26.4             pyhd3eb1b0_0  
        wheel                     0.36.2             pyhd3eb1b0_0  
        xz                        5.2.5                h7b6447c_0  
        yaml                      0.2.5                h7b6447c_0  
        zlib                      1.2.11               h7b6447c_3  


.. _condaConfiguration:

:program:`conda` Configuration
==============================

:program:`conda` uses configuration settings in your :file:`$HOME/.condarc` file to supplement its default configuration.
You need to set up this configuration on each machine that you use :program:`conda` on;
i.e. on your laptop,
and on the Waterhole workstation that you use
(which will cover all of the Waterhole/Ocean machines).

The :command:`conda config` command is how you interact with the :file:`$HOME/.condarc` file.
Start by telling :program:`conda` to use the `conda-forge`_ channel as its preferred channel to find packages:

.. _conda-forge: https://conda-forge.org/

.. code-block:: bash

    $ conda config --prepend channels conda-forge

Also do:

.. code-block:: bash

    $ conda config --prepend envs_dirs $HOME/conda_envs/
    $ mkdir $HOME/conda_envs/

The first of those lines tells :program:`conda` that you want to put your environments in a directory called :file:`$HOME/conda_envs/`.
The second line creates that directory.
Storing environment directory trees outside of the :file:`$HOME/miniconda3/` directory created by the installer means that if you need to re-install Miniconda you can do so without destroying all of your environments.

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
