.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MOAD-AnalysisRepository:

*********************
Analysis Repositories
*********************

Each member of the MOAD group has an *analysis repository*.
This section is about creating and using your personal analysis repository.

We each have an analysis repository so that we have a default place to store,
version control,
and share work,
mostly in the form of Jupyter Notebooks.
In time you will work in other repositories,
and create your own repositories for papers,
course work,
etc.,
but having a default place to do work helps keep things organized,
and helps other people find your work when you graduate and move on from MOAD to other adventures.
Most of the links you see people sharing on the weekly meeting whiteboard are to notebooks in their
analysis repositories that they have pushed to GitHub.

Our conventions are:

#. Analysis repositories are called ``analysis-firstname``;
   e.g. `analysis-susan`_

   .. _analysis-susan: https://github.com/SalishSeaCast/analysis-susan

#. Analysis repositories are public so that other researchers in the group,
   and outside of it can see the code and visualizations that you are creating,
   and learn from them


.. _SetUpAnalysisRepository:

Set Up Your Analysis Repository
===============================

The steps to set up your own analysis repository are:

#. Create an empty public repository on GitHub and clone it to your laptop or MOAD workstation
#. Use the MOAD `analysis repository cookiecutter`_ to generate the directory structure and
   initial files for your repository
#. Commit and push the initial files to GitHub
#. Create the conda environment to use for working in your analysis repository

.. _analysis repository cookiecutter: https://github.com/UBC-MOAD/cookiecutter-analysis-repo


Create Your Analysis Repository on GitHub
-----------------------------------------

#. In your browser,
   go to the `SalishSeaCast`_ GitHub organization page,
   and use the green :guilabel:`New` button to start creating your analysis repository.

   .. _SalishSeaCast: https://github.com/SalishSeaCast

#. Make sure the :guilabel:`Owner` selection box on the ``Create a new repository`` page shows
   the :guilabel:`SalishSeaCast` organization.

#. Type ``analysis-yourfirstname`` into the the :guilabel:`Repository name` text box;
   for example ``analysis-casey``.

#. Ensure the button to make your new repository ``Public`` is set.

#. Click the green :guilabel:`Create repository` button at the bottom of the page.

#. Keep the browser tab open because you are going to need information from it shortly.


.. _CloneYourAnalysisRepository:

Clone Your Analysis Repository
------------------------------

.. note::
    This section assumes that you have already followed that steps in the
    :ref:`SecureRemoteAccess` section to :ref:`GenerateSshKeys`,
    and to :ref:`CopyYourPublicSshKeyToGitHub`.

#. Create a top level directory for MOAD work.
   On your laptop do:

   .. code-block:: bash

       $ mkdir -p $HOME/MOAD

   On a Waterhole workstation do:

   .. code-block:: bash

       $ mkdir -p /ocean/$USER/MOAD

   The ``-p`` option tell :command:`mkdir` to not show an error message
   if the directory already exists,
   and to create any necessary parent directories as needed.

   :envvar:`$HOME` expands to your home directory.

   :envvar:`$USER` expands to your user name.

#. Go back to the browser tab in which you created your analysis repository on GitHub and find
   the section of the page near the top that says
   "Quick setup — if you’ve done this kind of thing before".
   Below that there are 2 buttons that say :guilabel:`HTTPS` and :guilabel:`SSH`.
   Please ensure that the :guilabel:`SSH` button is enabled,
   and copy the repository URI string of text beside it that looks like::

     git@github.com:SalishSeaCast/analysis-casey.git

#. Use that repository URI string to clone your analysis repository from GitHub.
   On your laptop do:

   .. code-block:: bash

       $ cd $HOME/MOAD
       $ git clone git@github.com:SalishSeaCast/analysis-casey.git

   On a Waterhole workstation do:

   .. code-block:: bash

       $ cd /ocean/$USER/MOAD
       $ git clone git@github.com:SalishSeaCast/analysis-casey.git


Populate Your Analysis Repository
---------------------------------

.. note::
    This section assumes that you have :ref:`Installed Miniforge <InstallingMiniforge>`
    on your laptop.

    It also assumes that you have set up your :ref:`GitConfiguration`.

.. note::
    You only need to do the steps in the section in the clone of your analysis repository
    on *either* your laptop *or* on a Waterhole machine.
    Once you have done these steps to create the basic directories and files in your repository,
    committed them in Git,
    and pushed them to GitHub,
    you can pull the changes from GitHub into other clones of your repository.

#. Create a :program:`conda` environment with the latest version of Python
   and the `cookiecutter tool`_ installed in it with the command:

   .. _cookiecutter tool: https://cookiecutter.readthedocs.io/en/latest/

   .. code-block:: bash

       $ conda create -n cookiecutter -c conda-forge python=3 cookiecutter

   That command will do some processing and then show you a list of packages
   that will be downloaded and installed,
   and ask you if it is okay to proceed;
   hit ``y`` or ``Enter`` to go ahead.

   After some more processing you should see the messages::

     Preparing transaction: done
     Verifying transaction: done
     Executing transaction: done
     #
     # To activate this environment, use
     #
     #     $ conda activate cookiecutter
     #
     # To deactivate an active environment, use
     #
     #     $ conda deactivate

#. Activate the ``cookiecutter`` environment,
   go to your :file:`MOAD/` directory,
   and populate your empty analysis repository clone with the commands:

   .. code-block:: bash

       $ conda activate cookiecutter
       (cookiecutter)$ cd $HOME/MOAD/
       (cookiecutter)$ cookiecutter -f gh:UBC-MOAD/cookiecutter-analysis-repo

   .. note::
      When you activate a conda environment the name of the environment in parentheses is
      added to the front of your command-line prompt.
      So,
      in the above commands,
      the command-line prompt changed from ``$``
      (or perhaps ``(base)$``)
      to ``(cookiecutter)$``.

   Those command use our `analysis repository cookiecutter`_ template repository
   to create directories and files in the empty analysis repository that you cloned earlier.
   The ``-f`` option lets the :command:`cookiecutter` tool write directories and files
   into an already existing directory.

   :command:`cookiecutter` will ask you for 2 pieces of input::

      researcher_name [Casey Lawrence]:
      Select github_org:
      1 - SalishSeaCast
      2 - UBC-MOAD
      3 - SS-Atlantis
      Choose from 1, 2, 3 [1]:

   Type your name in at the ``researcher_name`` prompt,
   and accept the default for ``github_org`` should match what you did earlier.

#. Deactivate your ``cookiecutter`` environment with:

   .. code-block:: bash

       (cookiecutter)$ conda deactivate

#. Go into your new analysis repository,
   add and commit the files that :command:`cookiecutter` created for you,
   and push them to GitHub:

   .. code-block:: bash

       $ cd $HOME/MOAD/analysis-casey
       $ git add .gitignore LICENSE README.rst notebooks/
       $ git commit -m "Initialize repo from MOAD cookiecutter"
       $ git push


Create Your Analysis Repository Conda Environment
-------------------------------------------------

.. note::
    This section assumes that you have :ref:`Installed Miniforge <InstallingMiniforge>`
    on whatever machine you are working on.

One of the files that :command:`cookiecutter` created for you is :file:`notebooks/environment.yaml`.
It is an environment description file that you use to tell :command:`conda` how to set up the
environment that you will use to work in your analysis repository.
That information includes things like the name of the environment,
the version of Python to install in it,
and the names of the Python packages to install in the environment.

#. Go into the :file:`notebooks/` directory of your analysis repository,
   and use :command:`conda` to create the environment:

   .. code-block:: bash

       $ cd $HOME/MOAD/analysis-casey/notebooks/
       $ conda env create -f environment.yaml

   As was the case when you created the ``cookiecutter`` environment above,
   that command will do some processing and then show you a list of packages
   that will be downloaded and installed,
   and ask you if it is okay to proceed;
   hit ``y`` or ``Enter`` to go ahead.

   After some more processing you should see messages like::

     Preparing transaction: done
     Verifying transaction: done
     Executing transaction: done
     #
     # To activate this environment, use
     #
     #     $ conda activate analysis-casey
     #
     # To deactivate an active environment, use
     #
     #     $ conda deactivate

Use the :command:`conda activate` command to activate your analysis environment so that you can
run :ref:`MOAD-Jupyter`.


Use Your Analysis Repository on Other Machines
----------------------------------------------

Once you have created your analysis repository and pushed it to GitHub you can clone it on other
machines,
create a conda environment work working in it,
and pull changes that you push to GitHub on one machine to update your repository on another machine.
