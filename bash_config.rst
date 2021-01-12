.. Copyright 2018-2021 The UBC EOAS MOAD Group
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

    echo $shell

If the shell you are using is :command:`bash`,
the output of that command will most likely be::

  /bin/bash

In order to make :command:`bash` work well for us on the Waterhole workstations and other MOAD computers,
we have to create two configuration files in our home directory:

#. :file:`.bash_profile`
#. :file:`.bashrc`

If you are new to :command:`bash` or the Linux command-line,
ask Doug for a link to the PDF of "The Linux Command Line" book by William E. Shotts, Jr.
For quick reminders about some of the most often used Linux commands this `Unix Shell Quick Reference`_ page from `Software Carpentry`_ is also helpful.

.. _Unix Shell Quick Reference: https://douglatornell.github.io/2013-09-26-ubc/lessons/ref/shell.html
.. _Software Carpentry: https://software-carpentry.org/


.. _Create-.bash_profile:

Create :file:`.bash_profile`
============================

New MOAD accounts on the Waterhole workstations are created completely empty.
So,
the first thing we need to do is create a file called `.bash_profile` in the home directory.
Your home directory is the one that you land in when you :command:`ssh` to a remote computer,
or when you start up a terminal session.

.. note::
    You only need to do this once on the MOAD machines because you home directory is shared across all of the machines.

    You probably don't need to do this on your laptop because :file:`.bash_profile` likely already exists,
    and you shouldn't change it until you know what you are doing and why.

Use the :command:`nano` text editor to create a new :command:`.bash_profile` file:

.. code-block:: bash

    $ nano .bash_profile

The :kbd:`.` at the beginning of the file name is important!
Type,
or copy/paste the following lines into the file:

.. code-block:: bash

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

Create :file:`.bashrc`
======================

.. note::
    You only need to do this once on the MOAD machines because you home directory is shared across all of the machines.

    You probably don't need to do this on your laptop because :file:`.bashrc` likely already exists.
    But if you like some of the aliases below you might want to add them to :file:`.bashrc` on your laptop.

Use the :command:`nano` text editor to create a new :command:`.bashrc` file:

.. code-block:: bash

    $ nano .bashrc

The :kbd:`.` at the beginning of the file name is important!
Type,
or copy/paste the following lines into the file:

.. code-block:: bash

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

TODO: Add explanation of each of the commands above.

When you are done,
save the file,
and exit :command:`nano`.

You will have to leave the shell by typing the command:

.. code-block:: bash

    $ exit

and then :command:`ssh` into the workstation again in order for :command:`bash` to use your new :file:`.bash_profile` and :file:`.bashrc` files.
