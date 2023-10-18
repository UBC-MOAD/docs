.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _bashConfiguration:

*****************************
:command:`bash` Configuration
*****************************

We do a lot of work
(especially when working remotely)
using the Linux command-line interface in terminal sessions.
All of our documentation assumes that the particular "flavour" of that interface you are using is :command:`bash`.
For historical reasons,
Linux and Unix command-line interfaces are called "shells".
So,
when someone talks about using :command:`bash`,
or "the shell",
they mean typing commands into a terminal session.

You can find out what shell you are using in most terminal sessions with the command:

.. code-block:: bash

    echo $SHELL

If the shell you are using is :command:`bash`,
the output of that command will most likely be::

  /bin/bash

In order to make :command:`bash` work well for us on the Waterhole workstations and other MOAD computers,
we have to create or edit two configuration files in our home directory:

#. :file:`.bash_profile`
#. :file:`.bashrc`

If you are new to :command:`bash` or the Linux command-line,
ask Doug for a link to the PDF of "The Linux Command Line" book by William E. Shotts, Jr.
For quick reminders about some of the most often used Linux commands this `Unix Shell Quick Reference`_ page from `Software Carpentry`_ is also helpful.

.. _Unix Shell Quick Reference: https://douglatornell.github.io/2013-09-26-ubc/lessons/ref/shell.html
.. _Software Carpentry: https://software-carpentry.org/


.. _Create-.bash_profile:

Edit or Create :file:`.bash_profile`
====================================

New MOAD accounts on the Waterhole workstations usually contain default versions 2 important configuration files,
but sometimes those files are not created.
:file:`.bash_profile` is one of those files.
So,
the first thing we need to do is edit that file,
or create it if it doesn't exist.
:file:`.bash_profile` is stored in your home directory.
Your home directory is the one that you land in when you :command:`ssh` to a remote computer,
or when you start up a terminal session.

.. note::
    You only need to do this once on the MOAD machines because you home directory is shared across all of the machines.

    You probably don't need to do this on your laptop because :file:`.bash_profile` likely already exists,
    and you shouldn't change it until you know what you are doing and why.

Use the :command:`nano` text editor to open the :file:`.bash_profile` file:

.. code-block:: bash

    $ nano .bash_profile

The ``.`` at the beginning of the file name is important!
Ensure that the file contains the following lines:

.. code-block:: bash

    # .bash_profile

    # Get the aliases and functions
    if [ -f "$HOME/.bashrc" ]; then
        . "$HOME/.bashrc"
    fi

    # set PATH so it includes user's private bin dirs if they exists
    if [ -d "$HOME/.local/bin" ] ; then
        PATH=$HOME/.local/bin:$PATH
    fi
    if [ -d "$HOME/bin" ] ; then
        PATH=$HOME/bin:$PATH
    fi

Then save the file,
and exit :command:`nano`.

When :command:`bash` starts up in a new terminal session it looks for the :file:`.bash_profile` file and executes the commands in it.
Those commands above tell :command:`bash` to check to see if another file called :file:`.bashrc` exists and,
if so,
execute the commands in it.
They also tell :command:`bash` about some user-specific directories that it should look in to find commands.

It will come as no surprise that the next thing we are going to do is :ref:`Create-.bashrc`.


.. _Create-.bashrc:

Edit or Create :file:`.bashrc`
==============================

The other important :command:`bash` configuration file is :file:`.bashrc`.

.. note::
    You only need to do this once on the MOAD machines because you home directory is shared across all of the machines.

    You probably don't need to do this on your laptop because :file:`.bashrc` likely already exists.
    But if you like some of the aliases below you might want to add them to :file:`.bashrc` on your laptop.

Use the :command:`nano` text editor to open the :command:`.bashrc` file:

.. code-block:: bash

    $ nano .bashrc

The ``.`` at the beginning of the file name is important!
Ensure that the file contains the following lines:

.. code-block:: bash

    # .bashrc

    # Source global definitions
    if [ -f /etc/bashrc ]; then
        . /etc/bashrc
    fi

    # Uncomment the following line if you don't like systemctl's auto-paging feature:
    # export SYSTEMD_PAGER=

    # User specific aliases and functions
    ## Environment variables
    # Shorter shell prompt
    PS1="\h:\W$ "

    # Pager setup
    export PAGER=less
    export LESS=-R

    # Make nano the default full-screen editor
    export EDITOR=nano
    export VISUAL=nano

    ## Aliases
    alias df="df -h"
    alias du="du -h"
    alias grep="grep --color=auto"
    alias ls="ls --color=auto -F"
    alias la="ls -a"
    alias ll="ls -al"
    alias rm="rm -i"

.. warning::
    :command:`alias ls="ls --color=auto -F"` does not work on MacOS.
    It produces an error.
    Instead,
    to get coloured output from :command:`ls` you have to set the :envvar:`CLICOLOR` environment variable to ``True`` by putting:

    .. code-block:: bash

         export CLICOLOR=True

    in your :file:`.bashrc` file.

When you are done,
save the file,
and exit :command:`nano`.

You will have to leave the shell by typing the command:

.. code-block:: bash

    $ exit

and then :command:`ssh` into the workstation again in order for :command:`bash` to use your new :file:`.bash_profile` and :file:`.bashrc` files.


.. _.bashrcCommandExplanations:

:file:`.bashrc` Command Explanations
------------------------------------

This section briefly explains what each of the command in the section above mean.

.. code-block:: bash

    PS1="\h:\W$ "

shortens your command-line prompt so that it shows just the name of the machine that you are on and the directory that you are currently in instead of the whole path to that directory.

.. code-block:: bash

    export PAGER=less

forces programs and commands that want to display output page by page to use :command:`less` as their pager.

.. code-block:: bash

    export LESS=-R

forces :command:`less` to allow control sequences that change the colour of its output

If you are not a fan of the :command:`vi` editor you can set the :envvar:`EDITOR` and :envvar:`VISUAL` environment variables to the command for your favourite editor and export them.
For :command:`emacs` use:

.. code-block:: bash

    export EDITOR=nano
    export VISUAL=nano

tells the system to use :command:`nano` as your default editor.
If you prefer a different editor,
substitute its name.
Other common choices are :command:`emacs`,
:command:`vim`,
or :command:`vi`.

.. code-block:: bash

    alias df="df -h"

modifies the :command:`df` command that shows how much disk space is free to use human-friendly units like ``G`` for gigabytes instead of its default of 1K blocks.

.. code-block:: bash

    alias du="du -h"

similarly modifies the :command:`du` command that shows disk space usage.

.. code-block:: bash

    alias grep="grep --color=auto"

modifies the :command:`grep` command for finding strings in files to show its output in colour.

.. code-block:: bash

    alias ls="ls --color=auto -F"

modifies the :command:`ls` command for listing directory contents to show its output in colour.
It also make :command:`ls` show extra characters after the file/directory names to indicate special properties;
e.g. append ``/`` to directory names,
``*`` to executable files,
``@`` to symbolic links,
etc.

.. code-block:: bash

    alias la="ls -a"

creates a new command,
:command:`la`,
that is an alias for :command:`ls -a` to make :command:`ls` show hidden files and directories
(whose names start with the ``.`` character).
Aliases are cumulative,
so,
:command:`la` will also be in colour and have appended indicator characters because of the way :command:`ls` is aliased in the line above.

.. code-block:: bash

    alias ll="ls -al"

creates a new command,
:command:`ll`,
that is an alias for :command:`ls -al` to make :command:`ls` show lots of details
(permissions,
owner and group,
file size,
and last modification date/time)
about files
(often called a long listing)
and include hidden files/directories in the listing.

.. code-block:: bash

    alias rm="rm -i"

modifies the :command:`rm` command for removing files to always prompt you to confirm that you really want to delete the file(s).

.. note::
  You can find more information about any of the commands in the aliases above by using the :command:`man` command;
  e.g. to find out more about the options available to control file and directory listings that the :command:`ls` produces,
  use :command:`man ls`,
  or Google something like "linux ls" or "man ls".
