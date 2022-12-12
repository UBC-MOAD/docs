.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _vc-with-hg:

******************************
Version Control with Mercurial
******************************

.. warning::

    We changed our version control tool to Git and moved hosting of our repositories to GitHub in early 2020 due to Bitbucket's ending of support for Mercurial.
    Please see :ref:`vc-with-git` for the up to date version of this section.


We use Mercurial_ (:command:`hg`) for version control of code,
documentation,
and pretty much any other important computer files in the Salish Sea MEOPAR project.

.. _Mercurial: https://www.mercurial-scm.org/

The Mercurial site includes `beginner's guides`_,
but `Mercurial - The Definitive Guide`_
(also known as "the redbean book") is the go-to reference.
If you are new to version control you should read at least Chapters 1_ and 2_.
Users experienced with other version control tools
(e.g :command:`svn` or :command:`git`)
can get up to speed with Mercurial by reading `Chapter 2`_.

.. _beginner's guides: https://www.mercurial-scm.org/wiki/BeginnersGuides
.. _Mercurial - The Definitive Guide: http://hgbook.red-bean.com/
.. _1: http://hgbook.red-bean.com/read/how-did-we-get-here.html
.. _2: http://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html
.. _Chapter 2: http://hgbook.red-bean.com/read/a-tour-of-mercurial-the-basics.html

The central storage of MOAD repositories is in various team accounts on `Bitbucket.org`_.
If you haven't done so already,
you should:

* Create a `Bitbucket.org`_ account.
  If you use an academic domain email address (like ``@eos.ubc.ca``) you will get perks like unlimited private repo collaboration.
* Send your Bitbucket user id to dlatornell@eos.ubc.ca so that you can be added to the appropriate team accounts.
* Follow the `Bitbucket ssh Set-up`_ instructions to enable :command:`ssh` key authentication.

  .. note::

      You only need to do the :command:`ssh-keygen` part described in step 3 once.
      Once you have an ssh key-pair you can use it from all of your working environments.

.. _Bitbucket.org: https://bitbucket.org/
.. _SalishSea-MEOPAR: https://bitbucket.org/salishsea/
.. _Bitbucket ssh Set-up: https://support.atlassian.com/bitbucket-cloud/docs/configure-ssh-and-two-step-verification/


Installing Mercurial
====================

Obviously,
you need to have Mercurial installed on your computer.
It is already installed on the Waterhole workstations,
and ``salish`` at UBC.
It is also installed on ``orcinus`` on WestGrid,
and ``beluga``,
``cedar``,
and ``graham`` on ComputeCanada.
If you have administrator privileges on your workstation or laptop you can download and install Mercurial for your operating system from https://www.mercurial-scm.org/downloads,
otherwise,
contact your IT support to have it installed for you.

Windows users may want to use TortoiseHg_ or SourceTree_,
GUI interface tools that integrate with Windows Explorer.
However,
this documentation focuses on command line use of Mercurial.
The workflows described below should be easily translatable into the GUI interface.
TortoiseHg also includes a command line interface.

.. _TortoiseHg: https://tortoisehg.bitbucket.io/
.. _SourceTree: https://www.sourcetreeapp.com/


.. _MercurialConfiguration:

Mercurial Configuration
=======================

Mercurial uses configuration settings in your :file:`$HOME/.hgrc` file as global settings for everything you do with it.
You need to set up this configuration on each machine that you use Mercurial on.
Do that with the command:

.. code-block:: bash

    EDITOR=nano hg config --edit

The first time you do that on a new machine you will see a template like:

.. code-block:: ini

    # example user config (see "hg help config" for more info)
    [ui]
    # name and email, e.g.
    # username = Jane Doe <jdoe@example.com>
    username =

    [extensions]
    # uncomment these lines to enable some popular extensions
    # (see "hg help extensions" for more info)
    #
    # pager =
    # color =

Replace that with:

.. code-block:: ini

    [alias]
    glog = log --graph

    [extensions]
    color =
    pager =
    progress =
    rebase =
    strip =

    [pager]
    pager = LESS='FRX' less

    [ui]
    username = Your Name <your_email_address>
    ignore = $HOME/.hgignore

Please to be sure to replace ``Your Name <your_email_address>`` with your name and email address.

The ``[alias]`` section defines command aliases within Mercurial:

* ``glog = log --graph`` make :command:`hg glog` an alias for the :command:`hg log --graph` command that formats the output as a graph representing the revision history using ASCII characters to the left of the log.
  That functionality and the :command:`hg glog` command were previously provided by the now-deprecated ``graphlog`` extension.

The ``[extensions]`` section enables several useful Mercurial extensions:

* ``color`` shows log listing,
  diffs,
  etc. in colour

* ``pager`` sends output of Mercurial commands through the pager that you specify in the ``[pager]``
  section so that long output is displayed one page at a time

* ``progress`` provides progress bars in the output of commands that are going to take more than a second or two to complete

* ``rebase`` enables rebasing which is particularly useful when working in repositories to which several contributors are pushing changes.
  As described below,
  ``rebase`` allows changes that have been pushed by other contributors to be pulled into your cloned repo while you have committed changes that have not been pushed without having to do frivolous branch merges.
  See :ref:`PullingAndRebaseingChangesFromUpstream` for more details.

* ``strip`` provides the ``strip`` command to remove changesets and their descendants from a repository.
  We very occasionally need to use this for repository maintenance.

The ``[ui]`` section configures the Mercurial user interface:

* ``username`` defines the name and email address that will be used in your commits.
  You should use the same email address as the one you have registered on Bitbucket.

* ``ignore`` is the path and name of an ignore file to be applied to all repositories
  (see :ref:`global-ignore-file`)

See the `Mercurial configuration file docs`_ for more information about configuration options.

.. _Mercurial configuration file docs: https://www.selenic.com/mercurial/hgrc.5.html


.. _global-ignore-file:

Global Ignore File
==================

Mercurial uses the file specified by ``ignore`` in the ``[ui]`` configuration section to define a set of ignore patterns that will be applied to all repos.
Like your Mercurial configuration,
you need to set this up on each machine that you use Mercurial on.
The recommended path and name for that file is :file:`$HOME/.hgignore`.

You should create or edit your :file:`$HOME/.hgignore` file to contain::

  syntax: glob
  *~
  *.pyc
  *.egg-info
  .ipynb_checkpoints
  .DS_Store
  .coverage
  .cache

  syntax: regexp
  (.*/)?\#[^/]*\#$
  ^docs/(.*)build/

The :``syntax: glob`` section uses shell wildcard expansion to define file patterns to be ignored.

The :``syntax: regexp`` section uses regular expressions to define ignore patterns.
The ``^docs/(.*)build/`` pattern ignores the products of Sphinx documentation builds in :file:`docs/` directories.

Most repos have their own :file:`.hgignore` file that defines patterns to ignore for that repo in addition to those specified globally.

See the `ignore file syntax docs`_ for more information.

.. _ignore file syntax docs: https://www.selenic.com/mercurial/hgignore.5.html


Mercurial Workflows
===================

.. note::

    Mercurial commands may be shortened to the fewest number of letters that uniquely identifies them.
    For example,
    :command:`hg status` can be spelled :command:`hg stat` or even :command:`hg st`.
    If you don't provide enough letters Mercurial will show the the possible command completions.


.. _PullingAndRebaseingChangesFromUpstream:

Pulling and Rebasing Changes from Upstream
------------------------------------------

The upstream Bitbucket repos from which you cloned your local working repos are the central repos to which everyone working on the project push their changes.
This section describes workflows for pulling those changes into your repos,
how to do so without having to do frivolous branch merges,
and how to recover from the common mistakes.

Use :command:`hg incoming` to see changes that are present in the upstream repo that have not yet been pulled into your local repo.
Similarly,
:command:`hg outgoing` will show you the changes that are present in your local repo that have not been pushed upstream.

Ensure that you have committed all of your changes before you pull new changes from upstream;
i.e.
:command:`hg status` should show nothing or a list of untracked files marked with the ``!`` character.

:command:`hg pull --rebase` will pull the changes from upstream and merge your locally committed changes on top of them.
Using :command:`rebase` avoids the creation of a new head
(aka a branch)
in your local repo and an unnecessary merge commit that results from the use of :command:`hg pull --update`.
That reserves branching and merging for the relatively rare occasions when temporarily divergent lines of development are actually required.

The `rebase extension docs`_ have more information and diagrams of what's going on in this `common rebase use case`_.

.. _rebase extension docs: https://www.mercurial-scm.org/wiki/RebaseExtension
.. _common rebase use case: https://www.mercurial-scm.org/wiki/RebaseExtension#Scenario_A


Rebasing an Accidental Branch
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Sooner or later you will accidentally create a branch in your local repo.
Using :command:`hg pull --rebase` with uncommitted changes and then committing those changes is one way that an accidental branch can happen.
:command:`hg glog` is a variant of the :command:`hg log` command that shows an ASCII-art graph of the commit tree to the left of the commit log,
providing a way of visualizing branches.

:command:`hg rebase` can be used to move the changes on an accidental branch to the tip of the repo.
See the `scenarios section`_ of the `rebase extension docs`_ for diagrams and rebase command options for moving branches around in various ways.

.. _scenarios section: https://www.mercurial-scm.org/wiki/RebaseExtension#Scenarios


Aborting a Merge
----------------

You may find yourself having followed Mercurial's workflow suggestions have having merged changes from upstream but then realizing that you really should have rebased.
At that point if you try to do almost anything other than commit the merge Mercurial will stop you with a message like::

  abort: outstanding uncommitted merges

You can use :command:`hg update --clean` to discard the uncommitted changes,
effectively aborting the merge
(and any other uncommitted changes you might have).
After that you should use :command:`hg glog` or :command:`hg heads` to examine your repo structure because you may well have an accidental branch that you will want to rebase.

Incidentally,
:command:`hg update --clean` can be used any time that you want to discard all uncommitted changes,
but be warned,
it does so without keeping a backup.
See :command:`hg revert` for a less destructive way of discarding changes on a file by file basis
(but note that :command:`hg revert` cannot be used to undo a merge).


Amending the Last Commit
------------------------

:command:`hg commit --amend` can be used to alter the last commit,
provided that it has not yet been pushed upstream.
This allows for correction or elaboration of the commit message,
inclusion of additional changes in the commit,
or addition of new files to the commit,
etc.


Commit Message Style
--------------------

Commit messages can be written on the command line with the :command:`hg commit -m` option with the message enclosed in double-quotes
(``*``);
e.g.

.. code-block:: bash

    hg commit -m"Add Salish Sea NEMO model quick-start section."

Assuming that you have the :envvar:`EDITOR` environment variable set :command:`hg commit` without the ``-m`` option will open your editor for you to write your commit message and the files to be committed will be shown in the editor.
Using your editor for commit message also makes it easy to write multi-line commit messages.

Here are recommendations for commit message style::

  Short (70 chars or less) summary sentence.

  More detailed explanatory text, if necessary.  Wrap it to about 72
  characters or so. The blank line separating the summary from the body
  is critical (unless you omit the body entirely).

  Write your commit message in the imperative: "Fix bug" and not "Fixed bug"
  or "Fixes bug."

  Further paragraphs come after blank lines.

  - Bullet points are okay, too

  - Typically a hyphen or asterisk is used for the bullet, followed by a
    single space, with blank lines in between

  - Use a hanging indent


.. _WorkAroundForCloningTimeout:

Work-around for Cloning Timeout
-------------------------------

If you have the problem of :command:`hg clone` commands repeatedly timing out
(as was experienced on ``beluga`` in the spring of 2019)
you may be able to work around the issue by cloning the first changeset of the repo from Bitbucket and then pulling the remaining changesets and updating your working copy in a separate step.
For example,
for the ``NEMO-3.6-code`` repo,
the process to do that is:

.. code-block:: bash

    hg clone --rev 1 ssh://hg@bitbucket.org/salishsea/nemo-3.6-code NEMO-3.6-code
    cd NEMO-3.6-code
    hg pull --update

We think that the root cause of the timeout during cloning is that after the initial repo bundle is downloaded from Bitbucket the local processing of the bundle is so slow on the $PROJECT file systems of some HPC clusters that Bitbucket assumes that the network connection being used for the cloning operation has failed.
Clone just the first changeset avoids the bundle download operation.
Subsequently pulling additional changesets requires a more continuous stream of network communication between the local system and Bitbucket.
That keeps the network connection alive,
and makes the process more robust to slow local file system operations.
The pulling operation is also incremental,
so if there is a timeout while it is in progress,
repeating the :command:`hg pull --update` command will resume at the point of failure instead of starting over again at the beginning.
