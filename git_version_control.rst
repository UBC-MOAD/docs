.. Copyright 2018-2021 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _vc-with-git:


************************
Version Control with Git
************************

We use Git_ for version control of code,
documentation,
and pretty much any other important computer files in the UBC MOAD group.

.. _Git: https://git-scm.com/

The `documentation section of the Git site`_ includes a variety of ways to learn about Git.
A good starting point is to read at least the first two chapters of the `Pro Git book`_.
There is also an introduction to `using Git`_ on the GitHub documentation site.

.. _documentation section of the Git site: https://git-scm.com/doc
.. _Pro Git book: https://git-scm.com/book/en/v2
.. _using Git: https://docs.github.com/en/get-started

We use `GitHub`_ for central storage of MOAD respositories to enable us to share our work with each other,
collaborators,
and the world at large.
If you have not already done so,
please :ref:`GetSetUpOnGitHub` so that Doug can invite you to our :ref:`GitHub organizations<team-repos>` to give you access to our public and private repositories.

.. _GitHub: https://github.com/

In addition to our GitHub organizations,
you also have a personal GitHub space.
Feel free to use that space for personal repositories,
course work,
etc.,
but please put your research repositories in the appropriate one of our GitHub organizations.


.. _InstallingGit:

Installing Git
==============

`Git`_ is a command-line tool that you need to have installed on your computer.
It is already installed on the Waterhole workstations,
and :kbd:`salish` at UBC.
It is also installed on :kbd:`graham` and the other ComputeCanada clusters.

On your laptop,
the installation method depends on your operating system.
Please follow the instructions for your OS on the `Git Downloads`_ page:

* For MacOS,
  trying the command :command:`git --version` will tell you what version of :program:`git` you have installed,
  or perhaps install it via :kbd:`Commandline Tools` for you.
  If it is not installed,
  `homebrew`_ is probably the best option,
  unless you already have Xcode installed.
* For Windows,
  the executable installer that downloads automatically should be straight-forward.
* For Linux,
  use your system package manager.

.. _Git Downloads: https://git-scm.com/downloads
.. _homebrew: https://brew.sh/


.. _GitConfiguration:

Git Configuration
=================

Git uses configuration settings in your :file:`$HOME/.gitconfig` file as global settings for everything you do with it.
You need to set up this configuration on each machine that you use Git on;
i.e. on your laptop,
on the Waterhole workstation that you use
(which will cover all of the Waterhole/Ocean machines),
and on :kbd:`graham`,
if you use it.

The :command:`git config --global` command is how you interact with the :file:`$HOME/.gitconfig` file.
Start by telling Git who you are.
It will include this information as part of the metadata in every commit you make.

.. code-block:: bash

    $ git config --global user.name "Your Name"
    $ git config --global user.email "you@example.com"

Also do:

.. code-block:: bash

    $ git config --global init.defaultbranch main
    $ git config --global pull.rebase false

The first of those lines tells Git that when you create a new repository with the :command:`git init` command,
it should call the default branch :kbd:`main`.
This is consistent with the default we have set on GitHub as of mid-2020.
Don't worry if you don't understand what "default branch" means;
you will learn soon.

The second tells Git to merge changes that it pulls in from remote repositories instead of rebasing them.
Again,
don't worry if you don't understand what that means right now;
you will learn.
We just have to tell Git what to do by default after :command:`git pull` so that it does not constantly tell us that we haven't specified a default.

If you want to see what is in your :file:`$HOME/.gitconfig` file,
you can use:

.. code-block:: bash

    git config --global --list

You can also have per-repository config files that are stored in the :file:`.git/config` file in a repo.
You interact with that file with :command:`git config --local`.
An example of when you might use that is to set a different email address from your EOAS one for a personal project repo.

There are many,
many things that you can configure in Git.
If you want to see all of the gory details,
please see the `git config docs`_.
The following sections delve into a few of those other configuration settings that you should consider using.

.. _git config docs: https://git-scm.com/docs/git-config


.. _GitCommitMessageEditor:

Commit Message Editor
---------------------

To write informative commit messages it is usually a good idea to have Git open your favourite text editor for you to type the message in.
By default,
Git opens :program:`vi`.
If you prefer to use a different editor,
you can tell Git that with:

.. code-block:: bash

    $ git config --global core.editor "your favourite editor"

where :kbd:`your favourite editor` is the command that Git should use to open your editor.
The commands for many popular editors are tabulated in `Appendix 3.1 of the ProGit book`_.
If you are having trouble figuring out what that command should be,
ask for help on the `SalishSeaCast #general`_ Slack channel.

.. _Appendix 3.1 of the ProGit book: https://git-scm.com/book/en/v2/Appendix-C%3A-Git-Commands-Setup-and-Config#ch_core_editor
.. _SalishSeaCast #general: https://salishseacast.slack.com/?redir=%2Farchives%2FCFR6VU70S


.. _GitCommandAliases:

Command Aliases
---------------

You can also use :command:`git config` to create aliases for complicated Git commands,
or commands that you want to give a short name to.
Here are some examples:

.. code-block:: bash

    $ git config --global alias.glog "log --graph"

This makes :command:`git glog` show you an ASCII-art graph version of the log of commit messages in a repo.
The graph shows branches have diverged and merged.
Mercurial users who relied on :command:`hg glog` will find this alias comforting.

.. code-block:: bash

    $ git config --global alias.out "log --pretty=oneline --abbrev-commit --graph @{u}.."

makes :command:`git out` show you the commits that you have made locally but not yet pushed to GitHub.
You can get more information about the changes in each of those commits by adding the :kbd:`--stat` option;
i.e. :command:`git out --stat`.

.. code-block:: bash

    $ git config --global alias.in '!git fetch && git log --pretty=oneline --abbrev-commit --graph ..@{u}'

makes :command:`git in` show you the commits from GitHub that have not yet been merged into your local repo.
Again,
adding the :kbd:`--stat` option adds information about the files that were changed in each commit and the number of added and deleted lines in each.


.. _GitIgnoreFiles:

:file:`.gitignore` Files
========================

There are lots of kinds of files that we *don't* generally want to track with :command:`git`,
and,
in fact,
don't even want :command:`git` to show us that they are untracked.
Examples include:

* byte-compiled Python files that are stored in :file:`__pycache__/` directories,
  or have extensions like :file:`.pyc`
* Jupyter notebook checkpoint files stored in :file:`.ipynb_checkpoints/` directories
* rendered Sphinx documentation files stored in :file:`_build/` directories
* etc.

A list of patterns in a file called :file:`.gitignore` in the top level directory of your repository tells :command:`git` to never show files that match those patterns in the :command:`git status` command so that they don't clutter up your view of changes that you need to add/commit.

There is more information about :file:`.gitignore` files in the "Ignoring Files" section of https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository.

You can create your own :file:`.gitignore` files by hand,
or use a template like https://github.com/github/gitignore/blob/master/Python.gitignore as a starting point.

.. note::
    Be sure to add and commit your :file:`.gitignore` file after you create it,
    and any time you change it.
