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

#. Create an empty public repository on GitHub and clone it on ``salish`` or one of the MOAD workstations
#. Use the MOAD `analysis repository cookiecutter`_ to generate the directory structure and
   initial files for your repository
#. Use Pixi to install Python and the packages needed to start working in your analysis repository
#. Commit and push the initial files to GitHub

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

#. On ``salish`` or a Waterhole workstation,
   create a top level directory for MOAD work:

   .. code-block:: console

       $ mkdir -p /ocean/$USER/MOAD

   The ``-p`` option tell :command:`mkdir` to not show an error message
   if the directory already exists,
   and to create any necessary parent directories as needed.

   :envvar:`$USER` expands to your user name.

#. Go back to the browser tab in which you created your analysis repository on GitHub and find
   the section of the page near the top that says
   "Quick setup — if you've done this kind of thing before".
   Below that there are 2 buttons that say :guilabel:`HTTPS` and :guilabel:`SSH`.
   Please ensure that the :guilabel:`SSH` button is enabled,
   and copy the repository URI string of text beside it that looks like:

   .. code-block:: output
      :class: no-copybutton

      git@github.com:SalishSeaCast/analysis-casey.git

   but with your name instead of ``casey`` in the repository URI string.

#. Use that repository URI string to clone your analysis repository from GitHub:

   .. code-block:: console

       $ cd /ocean/$USER/MOAD
       $ git clone git@github.com:SalishSeaCast/analysis-casey.git

   replacing ``git@github.com:SalishSeaCast/analysis-casey.git`` with the repository URI string that
   you copied from GitHub.


Populate Your Analysis Repository
---------------------------------

.. note::
    This section assumes that you have :ref:`Installed Pixi <InstallingPixi>`.

    It also assumes that you have set up your :ref:`GitConfiguration`.

.. note::
    You only need to do the steps in this section once in the clone of your analysis repository
    on ``salish`` or a Waterhole machine.
    After you have done these steps to create the directories and files in your repository,
    committed them in Git,
    and pushed them to GitHub,
    you can pull the changes from GitHub into clones of your repository on the Alliance HPC clusters
    or your laptop.

#. Go to your :file:`MOAD/` directory,
   and populate your empty analysis repository clone with the following commands:

   .. code-block:: console

       $ cd /ocean/$USER/MOAD
       $ pixi exec cookiecutter -f gh:UBC-MOAD/cookiecutter-analysis-repo

   Those command use our `analysis repository cookiecutter`_ template repository
   to create directories and files in the empty analysis repository that you cloned earlier.
   The ``-f`` option lets the :command:`cookiecutter` tool write directories and files
   into an already existing directory.

   :command:`cookiecutter` will ask you for 2 pieces of input:

   .. code-block:: output
      :class: no-copybutton

      researcher_name [Casey Lawrence]:
      Select github_org:
      1 - SalishSeaCast
      2 - UBC-MOAD
      3 - SS-Atlantis
      Choose from 1, 2, 3 [1]:

   Type your name in at the ``researcher_name`` prompt,
   and accept the default ``1`` for ``github_org`` so that :command:`cookiecutter` set things up
   to use your repository in the the `SalishSeaCast`_ GitHub organization.

#. Go into your new analysis repository,
   use Pixi to install the Python packages that are needed to work in it,
   add and commit the files that :command:`cookiecutter` and Pixi created for you,
   and push them to GitHub.

   .. code-block:: console

       $ cd /ocean/$USER/MOAD/analysis-casey
       $ pixi install
       $ git add .gitattributes .gitignore pixi.toml pixi.lock LICENSE README.rst notebooks/
       $ git commit -m "Initialize repo from MOAD cookiecutter"
       $ git push


Use Your Analysis Repository on Other Machines
----------------------------------------------

After you have created your analysis repository and pushed it to GitHub you can clone it on other
machines,
and set up the environment for working in it with `pixi install`.
Then,
when you make changes to your repository on one machine and push those changes to GitHub,
you can pull those changes on another machine to update the repository there.
