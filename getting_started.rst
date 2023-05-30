.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _GettingStarted:

***************
Getting Started
***************

This section is for people joining the MOAD group.
It's mostly a collection of links to other places in these and other docs in approximately the order that you need to do things to get up and running in the group.

There's lots to learn as you join the group,
but you can also help us improve things right from Day 1.
You are reading these docs with eyes and brain that haven't looked at them before,
so you are uniquely qualified to see things that other people take for granted.
Please help us improve our docs fixing typos,
improving wording,
etc. by using the :guilabel:`Edit on GitHub` link in the upper right corner of every page
(on a desktop browser - sadly that feature is not available at the moment on phones and tablets).
If you find something that you don't feel confident fixing,
please create an issue about it in the `issue tracker on GitHub`_.

.. _issue tracker on GitHub: https://github.com/UBC-MOAD/docs/issues

Here's the Getting Started checklist:

#. :ref:`GetYourEOASEmailAddressAndUserId`
#. :ref:`GetSetUpOnGitHub`
#. :ref:`SetUpSecureRemoteAccess`
#. :ref:`SetUpGit`
#. :ref:`SetUpBash`
#. :ref:`InstallMiniforge`
#. :ref:`SetUpYourAnalysisRepository`


.. _GetYourEOASEmailAddressAndUserId:

Get Your EOAS Email Address and User Id
=======================================

Susan will take you through the form to get your EOAS email address and Linux computer user id setup during your first meeting with her when you join the group.


.. _GetSetUpOnGitHub:

Get Set Up on GitHub
====================

If you don't already have an account on `GitHub`_,
please create one.
Then,
send your GitHub username to Doug at ``dlatornell@eoas.ubc.ca``.
He will add you to the `UBC-MOAD GitHub organization`_ and you will receive an invitation email from GitHub that you will have to accept in order to finalize your addition to the organization.
Doug will probably also invite you to at least one of the other project-specific :ref:`GitHub organizations<team-repos>` that we have.

.. _GitHub: https://github.com/
.. _UBC-MOAD GitHub organization: https://github.com/UBC-MOAD


.. _SetUpSecureRemoteAccess:

Set Up Secure Remote Access
===========================

You will need to have:

#. An account,
   user id,
   and password on the EOAS Ocean collection of Linux computers
   (which should happen at the same time as getting your ``eoas.ubc.ca`` email address)

before you can set up your :command:`ssh` keys and configuration for :ref:`SecureRemoteAccess`.


.. _SetUpGit:

Set Up Git
==========

You will need to:

#. Learn about :ref:`vc-with-git`
#. :ref:`Install Git<InstallingGit>` on your laptop
#. Set up your :ref:`GitConfiguration` on each of the machines you use


.. _SetUpBash:

Set Up :program:`bash`
======================

You will need to have:

#. A user id on the EOAS Ocean collection of Linux computers
   (which should happen at the same time as getting your ``eoas.ubc.ca`` email address)
#. Completed the process of :ref:`copying your public ssh key to a Waterhole workstation <CopyYourPublicSshKeyToRemoteComputers>`

before you can:

#. :ref:`Create-.bash_profile`
#. :ref:`Create-.bashrc`

on a Waterhole workstation.


.. _InstallMiniforge:

Install Miniforge
=================

You will need to:

#. Learn about :ref:`MOAD-CondaPkgAndEnvMgr`
#. :ref:`Install Miniforge<InstallingMiniforge>` on your laptop
#. :ref:`Install Miniforge<InstallingMiniforge>` in your workspace on the EOAS Ocean collection of Linux computers


.. _SetUpYourAnalysisRepository:

Set Up Your Analysis Repository
===============================

You will need to have:

#. :ref:`SetUpGit`
#. :ref:`Installed Miniconda<InstallMiniforge>`

before you can set up your :ref:`analysis repository<MOAD-AnalysisRepository>`.
