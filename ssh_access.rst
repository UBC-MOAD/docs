.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _SecureRemoteAccess:

********************
Secure Remote Access
********************

This section is the about setting up secure,
easy to use,
terminal (command-line) access from your laptop to the EOAS Ocean Linux computers.

The software technology we use to do that is called ``ssh`` (sometime lowercase, sometimes uppercase).
:command:`ssh` is also the command that makes terminal/command-line connections between computers.
:command:`ssh` stands for "secure shell".
The first ``s`` is for secure,
and the ``sh`` is in tribute to the original Unix command-line interface called "shell".
That's why people often say or write "open a shell" or "go to shell" when they mean open or switch to your Terminal program.

This brief `Introduction to SSH`_ explains some of the concepts and terminology about the software that we use to accomplish easy,
secure access.

.. _Introduction to SSH: https://www.baeldung.com/cs/ssh-intro

Specifically,
we use key-based authentication.
The `digitalocean.com ssh tutorial`_ has a good explanation of how key-based authentication works in comparison to password authentication:

  An SSH server can authenticate clients using a variety of different methods. The most basic of these is password authentication, which is easy to use, but not the most secure.

  Although passwords are sent to the server in a secure manner, they are generally not complex or long enough to be resistant to repeated, persistent attackers. Modern processing power combined with automated scripts make brute forcing a password-protected account very possible. Although there are other methods of adding additional security (fail2ban, etc.), SSH keys prove to be a reliable and secure alternative.

  SSH key pairs are two cryptographically secure keys that can be used to authenticate a client to an SSH server. Each key pair consists of a public key and a private key.

  The private key is retained by the client and should be kept absolutely secret. Any compromise of the private key will allow the attacker to log into servers that are configured with the associated public key without additional authentication. As an additional precaution, the key can be encrypted on disk with a passphrase.

  The associated public key can be shared freely without any negative consequences. The public key can be used to encrypt messages that only the private key can decrypt. This property is employed as a way of authenticating using the key pair.

  The public key is uploaded to a remote server that you want to be able to log into with SSH. The key is added to a special file within the user account you will be logging into called ~/.ssh/authorized_keys.

  When a client attempts to authenticate using SSH keys, the server can test the client on whether they are in possession of the private key. If the client can prove that it owns the private key, a shell session is spawned or the requested command is executed.

.. _digitalocean.com ssh tutorial: https://www.digitalocean.com/community/tutorials/how-to-configure-ssh-key-based-authentication-on-a-linux-server

The security of key-based authentication comes from:

* the cryptographic strength of key pair
* the length and complexity of the passphrase that you choose to use to encrypt your private key when it is stored on disk
* the fact that you remember that passphrase and don't write it down or share it with anyone

The thing that makes key-based authentication easy to use is a program called an "ssh agent" that runs on your laptop and manages using your keys to authenticate your access to remote systems.
Once you have your ssh keys set up,
the only time you should need to type in your passphrase is when you connect to a remote computer for the first time after your laptop has been started up after a shutdown or reboot.
When you connect to a remote computer that first time,
your ssh agent asks you for your passphrase so that it can decrypt your private key that is stored on disk.
After that,
it can use that key to do any authentications you need done,
until you shutdown or reboot your computer,
or explicitly tell the ssh agent to forget about a key.

It's not just authentications that are encrypted by ssh keys.
Once you sigh-in to a remote computer using ssh key-based authentication,
all of the information sent both ways over the Internet between your laptop and that computer is also encrypted.

The instructions below assume that your are working on a Mac or a Linux laptop or desktop.
They may also work on a Windows computer that is running Windows Subsystem for Linux (WSL),
but that has not been tested.
If you are using Windows 10,
you will need to install the :program:`Open SSH Client` app via
:guilabel:`Settings > Apps > Apps > Features > Optional Features`.
Please see `these instructions`_,
but *only* install the "Client" part – don't try to do any of the "server" parts.

.. _these instructions: https://learn.microsoft.com/en-ca/windows-server/administration/openssh/openssh_install_firstuse


.. note:: **Help Wanted**

    If you know about setting up ssh on Windows,
    please feel free to add to these docs,
    or contact Doug to help him do so.


.. _GenerateSshKeys:

Generate ssh Keys
=================

The first step is to generate an ssh key pair on you local computer.
That is done using the :command:`ssh-keygen` command.
Here we tell it to use the Ed25519 algorithm to create the keys.

Open your Terminal program to get a command-line interface,
and type the command:

.. code-block:: bash

    $ ssh-keygen -t ed25519

The output should look like:

.. code-block:: text

    Generating public/private rsa key pair.
    Enter file in which to save the key (/home/username/.ssh/id_ed25519):

but with with your laptop user id instead of ``username`` in the key file path in the parentheses.

Hit enter to accept the default key file path and name.

.. warning::

    If you get a message like:

    .. code-block:: text

        /home/username/.ssh/id_ed25519 already exists.
        Overwrite (y/n)?

    it means that you already have a key pair with the default name.
    Please use :kbd:`Ctrl-c` to exit from :command:`ssh-keygen` and contact Doug for advice on how to proceed.

You might see the message:

.. code-block:: text

    Created directory '/home/username/.ssh'.

next,
then,
for sure,
you should see the message:

.. code-block:: text

    Enter passphrase (empty for no passphrase):

This is where you enter the passphrase that encrypts your private key that will be stored on disk.
Please use a long passphrase that you can easily remember;
something like:

* a line or two from a favourite song, poem, or story
* a nonsense rhyme
* the names of some or all of your cousins
* a list of place names that are significant to you but that would be hard for someone else to guess
* a statement that someone in your family often makes
* a famous or inspiring quotation
* the names of the 12 goldfish you had growing up

You can use all the spaces and special characters that you want.
Remember that a longer passphrase is more secure,
and that you won't have to type it very often...

But you will have to type it again after the next prompt :-)

.. code-block:: text

    Enter same passphrase again:

When the key pair generation is finished you should see output like:

.. code-block:: text

    Your identification has been saved in /home/username/.ssh/id_ed25519
    Your public key has been saved in /home/username/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:8lYuN0DcZra83nBgsElzsP6EYqZYEt7zzslgKhuuxT8 username@host
    The key's randomart image is:
    +---[RSA 4096]----+
    |        .        |
    |       . +       |
    |  .     B *      |
    | . o   + % .     |
    |  o + = S B      |
    | . + * + B o     |
    |. + + . + B .    |
    |oo +E= o + =     |
    |++. ..=   . .    |
    +----[SHA256]-----+

except that you will see:

* your laptop user id instead of ``username`` in the key files paths
* a different key fingerprint,
  ending with your user id and computer name instead of ``username@host``
* a different "randomart image"

Congratulations!
You are now the owner of a shiny new ssh key pair!


.. _SetUpSshConfiguration:

Set Up ssh Configuration
========================

The next step to making ssh keys easy to use is to tell the :command:`ssh` command
(and other related commands)
how we want them to behave.
We do that by putting directives in the file :file:`/home/username/.ssh/config`.
You can use any text editor you want to do this:
:program:`vim`,
:program:`emacs`,
:program:`VSCode`,
:program:`SublimeText`,
:program:`Notepad`,
or :program:`nano`.

Please don't use a word processing program like Microsoft Word or LibreOffice Write.

We'll use :command:`nano` here because it is available almost everywhere.

Open the file with :command:`nano`:

.. code-block:: bash

    $ nano ~/.ssh/config

This should create a new,
empty file.
If there is already some text in the file,
please contact Doug for advise on how to proceed.

By the way,
:file:`~` is a shorthand way of typing :file:`/home/username` with your user id in place of ``username``.


Directives for All Hosts
------------------------

Put the following lines in your :file:`~/.ssh/config` file:

.. code-block:: text

    Host *
      ForwardAgent yes
      ServerAliveInterval 60

The first line,
``Host *``,
means that the directives that come next,
indented beneath it,
apply to all remote computers that you connect to.

The indentation can be spaces or tabs;
2 or 4 spaces are conventional.

The 2nd line,
``ForwardAgent yes``,
means that when you connect to a remote computer,
your ssh agent should set things up so that,
if you connect to another computer from that one,
the authentication handling is passed back to your laptop.
That means that you don't have to store copies of your private key on a bunch of different computers,
it can stay safe on your laptop.

The 3rd line,
``ServerAliveInterval 60``,
tells :command:`ssh` to send a keep-alive message every 60 seconds to any computer that you are connected to.
That helps prevent you connection from getting broken if you stop typing for a few minutes to think,
answer the phone,
check Slack,
or go make tea.
Some Internet service providers (notably Shaw) are really aggressive about shutting down idle network connections.
This directive helps a little to defend against that annoyance.

If you are working on a Mac with the Sierra or later version of the operating system, or if you are on Windows 11,
you should add another line to the stanza that you have already typed so that it looks like:

.. code-block:: text

    Host *
      ForwardAgent yes
      ServerAliveInterval 60
      AddKeysToAgent yes

That 4th line,
``AddKeysToAgent yes``,
tells the ssh agent to remember the keys that you give the passphrases for.
Apple decided to make Sierra and later versions of their OS super annoying
(though, admittedly, more secure - to the level of paranoia :-)
by making the ssh agent forgetful by default.


Host Aliases
------------

To make it easier to connect to remote systems that you use often you can add stanzas to your :file:`~/.ssh/config` that:

* give the remote computer a shorter name
* tell :command:`ssh` and friends what user id you use on that computer
* provide directives to override or augment those in the ``Host *`` stanza,
  or other defaults

Add host alias stanzas for the MOAD workstation that Susan told you to work on
(using ``chum`` here as an example),
and one for our compute server,
``salish``:

.. code-block:: text

    Host chum
      HostName chum.eos.ubc.ca
      User username

    Host salish
      HostName salish.eos.ubc.ca
      User username

.. note:: Please be sure to replace ``username`` with your EOAS user id.

It is conventional to separate the stanzas in :file:`~/.ssh/config` with empty lines.
You can also add comment lines if you want by starting them with the ``#`` character.
Your file should now look like:

.. code-block:: text

    Host *
      ForwardAgent yes
      ServerAliveInterval 60

    Host chum
      HostName chum.eos.ubc.ca
      User username

    Host salish
      HostName salish.eos.ubc.ca
      User username

Now,
instead of having to type:

.. code-block:: text

    ssh username@salish.eos.ubc.ca

you will be able to type:

.. code-block:: text

    ssh salish

(after we complete 1 more step of setup).

Save your file with :kbd:`Ctrl-o` (then hit enter),
and exit :program:`nano` with :kbd:`Ctrl-x`.


.. _CopyYourPublicSshKeyToRemoteComputers:

Copy Your Public ssh Key to Remote Computers
============================================

The final step to make :command:`ssh` key pair authentication work is to copy your public key to each remote system that you want to connect to.
The command to do that is :command:`ssh-copy-id`.

Copy your public key to ``salish`` with:

.. code-block:: bash

    $ ssh-copy-id salish

That command will use the information you put into :file:`~/.ssh/config` to expand ``salish`` to ``username@salish.eos.ca``.
It should produce output like:

.. code-block:: text

    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/username/.ssh/id_ed25519.pub"
    The authenticity of host 'salish.eos.ca (142.103.36.12)' can't be established.
    ecdsa-sha2-nistp256 key fingerprint is SHA256:W8No4t+8TJWxURHmsoOBwB0LYu1SFiLkNnxDrmsCS9I.
    Are you sure you want to continue connecting (yes/no/[fingerprint])?

Type ``yes`` to proceed.

The output from :command:`ssh-copy-id` should continue with:

.. code-block:: text

    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    username@salish.eos.ca's password:

Type in your EOAS Linux systems password sent to you by EOAS IT,
and the output from :command:`ssh-copy-id` should finish with:

.. code-block:: text

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh salish"
    and check to make sure that only the key(s) you wanted were added.

Now,
as the output says,
test the authentication with:

.. code-block:: bash

    $ ssh salish

Your ssh agent should ask you for your passphrase so that it can decrypt your private key,
then you should find yourself at the command-line prompt on ``salish``:

.. code-block:: text

    Welcome to Ubuntu 18.04.6 LTS (GNU/Linux 5.4.0-121-generic x86_64)

    * Documentation:  https://help.ubuntu.com
    * Management:     https://landscape.canonical.com
    * Support:        https://ubuntu.com/advantage

    21 updates can be applied immediately.
    To see these additional updates run: apt list --upgradable

    New release '20.04.6 LTS' available.
    Run 'do-release-upgrade' to upgrade to it.

    Your Hardware Enablement Stack (HWE) is supported until April 2023.
    ~$

Disconnect from ``salish`` with ``exit``,
and connect again with ``ssh salish``.
This time you should connect without being asked for your password or your passphrase.

.. note::

    You only have to copy your public key to one of the EOAS Ocean machines or MOAD workstations,
    not all of them.
    They all use the same authentication system,
    so what one knows,
    they all know.

If you are curious about what :command:`ssh-copy-id` is doing,
it is automating a bunch of steps to store your public key in a file called :file:`~/.ssh/authorized_keys`.
We used to have to do those steps one by one.
Life is much better with :command:`ssh-copy-id`...


.. _CopyYourPublicSshKeyToGitHub:

Copy Your Public ssh Key to GitHub
==================================

You can,
and should,
also use key-based authentication to access our :ref:`team-repos` and your personal repositories on GitHub.
Doing so lets your ssh agent handle authentication when you do :command:`git pull` and :command:`git push` commands to copy committed changes between your local repository clones and GitHub.
Please follow the `instructions provided by GitHub`_ to put your public key into your account settings on GitHub.

.. _instructions provided by GitHub: https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account


.. _SSHCommands:

SSH Commands
============

There is a collection of commands associated with SSH:

The ones you used above to get things set up:

* :command:`ssh-keygen` - generate and work with authentication key pairs for SSH
* :command:`ssh-copy-id` - copy a local public key to a remote computer so that SSH key pair authentication can be used to log in

The one you will most often use:

:command:`ssh` - log into a remote machine, or execute commands on a remote machine

Commands for copying files to/from/among your local computer and remote machines:

* :command:`scp` - an SSH-secured version of the :command:`cp` command that lets you copy files from one machine to another;
  local to/from remote,
  or remote to remote
* :command:`sftp` - an SSH-secured version of the FTP file transfer protocol that provides a command-line interface for doing things such as navigating and creating directories on remote machines, and copying files between your local file system and that of a remote machine

To get a short reminder of the option flags for any of these commands
(or most any Linux command),
use the ``--help`` option;
e.g.

.. code-block:: bash

    $ ssh --help

To get a detailed description of a command
(again, this works for most any Linux command)
use the :command:`man` command;
e.g.

.. code-block:: bash

    $ man scp

or use Google or another search engine in your browser.
Searching for "man scp" should give you hits for nicely formatted versions of the same information that :command:`man scp` gives you;
e.g. https://linux.die.net/man/1/scp

The :command:`man` command is short for "manual",
and the information it shows you is known as a "manpage".


.. _sshCommand:

:command:`ssh` Command
----------------------

Most often you will use :command:`ssh` to open a terminal session
(also knowns command-line interface, CLI, or shell)
on a remote computer;
e.g.

.. code-block:: bash

    $ ssh salish

You can also use :command:`ssh` to execute a command on a remote computer without actually opening the terminal session;
e.g.

.. code-block:: bash

    $ ssh salish ls -lh /results2/SalishSea/nowcast-green.201905/09sep20/

That command means:
"Use :command:`ssh` to connect to ``salish`` and show me the long listing
(including permissions, ownership, human-readable sizes, and date/time stamps)
of the files in the directory there called :file:`/results2/SalishSea/nowcast-green.201905/09sep20/`".

If you get too fancy with the command that you want to execute remotely you may have to enclose it in quotes to prevent your local shell from messing it up;
e.g.

.. code-block:: bash

    $ ssh salish "find /results/forcing/atmospheric/GEM2.5/GRIB/20200909/12 -type f | wc -l"

If you have trouble with :command:`ssh` not making a connection,
you can tell it to output debugging messages its progress by using the ``-v`` option;
e.g.

.. code-block:: bash

    $ ssh -v salish

This is helpful in debugging connection, authentication, and configuration problems.
Adding more ``v``s
(up to 3) e.g. ``-vv``,
or ``-vvv``,
increases the verbosity of the messages.
If you need help interpreting the output of :command:`ssh -v`,
paste it into a message on the `SalishSeaCast #general`_ Slack channel.

.. _SalishSeaCast #general: https://salishseacast.slack.com/?redir=%2Farchives%2FCFR6VU70S

Please see :command:`ssh --help`,
:command:`man ssh`,
ask on the `SalishSeaCast #general`_ Slack channel,
or Google for more information about how to use :command:`ssh`.


.. _scpCommand:

:command:`scp` Command
----------------------

:command:`scp` lets you copy one or more files between your local machine and a remove machine,
or between two remote machines without bringing the file to your local machine.

.. note::

    If you need to copy files between two Compute Canada cluster you should use :ref:`Globus-docs` because it is at least 4 times faster than :command:`scp` on the high performance network connections among the Compute Canada clusters.

To copy a file from your current directory on your local computer to your home directory on ``salish`` use:

.. code-block:: bash

    $ scp my-local-file salish:

You can also include a path in place of :file:`my-local-file`,
and/or after the colon in ``salish:``.
If you give several paths/files in place of :file:`my-local-file`,
all of those files will get copied to ``salish``.

To copy a file from your current directory on your local computer to your ``/ocean/$USER/`` space on :command:`salish` use:

.. code-block:: bash

    $ scp my-local-file "salish:/ocean/$USER/"

The quotes around ``"salish:/ocean/$USER/"`` are necessary to prevent your local shell from expanding the :envvar:`USER` environment variable.

To copy a file from your ``/ocean/$USER/`` space on ``salish`` to your current directory on your local computer use:

.. code-block:: bash

    $ scp "salish:/ocean/$USER/my-remote-file" ./

The :file:`./` at the end means,
this directory.
It could also be some other directory path.
Unlike :command:`cp`,
:command:`scp` always has to have a destination directory for the file.
Including a file name in the destination is an easy way to combine copying and renaming the copied file in one operation.

If you have trouble with :command:`scp` not making a connection,
you can tell it to output debugging messages its progress by using the ``-v`` option;
e.g.

.. code-block:: bash

    $ scp -v my-local-file salish:

This is helpful in debugging connection, authentication, and configuration problems.
If you need help interpreting the output of :command:`scp -v`,
paste it into a message on the `SalishSeaCast #general`_ Slack channel.

Please see :command:`scp --help`,
:command:`man scp`,
ask on the `SalishSeaCast #general`_ Slack channel,
or Google for more information about how to use :command:`scp`.


.. _sftpCommand:

:command:`sftp` Command
-----------------------

:command:`sftp` can be used to do the same job of copying files to/form remote machines as :command:`scp`.
But it also provides a command-line interface to do other operations on the remote and local file systems,
such as navigating directories
(``cd`` and ``lcd``),
and creating directories on the remote file system (``mkdir``).
The command for uploading a file is ``put``,
and for downloading ``get``.
``help`` or ``?`` will give you a list of the available command.

Please see :command:`sftp --help`,
:command:`man sftp`,
ask on the `SalishSeaCast #general`_ Slack channel,
or Google for more information about how to use :command:`sftp`.

Here is a sample :command:`sftp` session to copy a file from your scratch space on ``salish`` to your current directory on your local computer:

.. code-block:: text

    $ sftp salish
    Connected to salish.
    sftp> cd /ocean/username/
    sftp> get my-remote-file
    get my-remote-file
    Fetching /home/username/my-remote-file to my-remote-file
    /home/username/my-remote-file                 100%  954     7.0KB/s   00:00
    sftp> quit

If you have trouble with :command:`sftp` not making a connection,
you can tell it to output debugging messages its progress by using the ``-v`` option;
e.g.

.. code-block:: bash

    $ sftp -v salish

This is helpful in debugging connection, authentication, and configuration problems.
If you need help interpreting the output of :command:`sftp -v`,
paste it into a message on the `SalishSeaCast #general`_ Slack channel.


.. _X2GoRemoteDesktop:

X2Go Remote Desktop
===================

A last resort for remote access to MOAD workstations is to use X2Go
(``https://wiki.x2go.org/doku.php``).
It is a last resort because it is very bandwidth-hungry,
so it can be painfully slow to use on home WiFi.
It also relies on password authentication rather than key-based authentication,
so it is less secure.
Once authentication is completed X2Go uses SSH to secure all of the data between your local machine and the remote one.

.. _X2Go: https://wiki.x2go.org/doku.php

.. warning::

    Please be particularly cautious of X2Go's weaker authentication security on public Wifi connections such as coffee shops,
    or public libraries.
    Since bandwidth on those types of connections is often very limited,
    X2Go is likely an all-round bad choice on public WiFi.

One use case that X2Go is helpful for is accessing UBC sites like the payroll system that require VPN for access from outside the UBC network.
Connecting to a MOAD workstation desktop puts you inside the UBC network,
so you can use Firefox on that desktop to connect to UBC the payroll system without the need for VPN on your laptop.

Please see the X2Go documentation for instructions on how to install the X2Go client for your operating system.

Once you have the client installed and running,
use the :guilabel:`Session` menu to create a :guilabel:`New Session` for a specific workstation;
we'll use ``chum`` as an example.

In the :guilabel:`Session preferences` dialog enter:

* :guilabel:`Session name:` ``chum``
* :guilabel:`Host:` ``chum.eos.ubc.ca``
* :guilabel:`Login`: Your EOAS user id
* :guilabel:`Session type` drop-down: choose ``Mate``

After you click the :guilabel:`Okay` button you should see a new session tile called :guilabel:`chum` on the right side of the X2Go window.

.. note::

    Finding a :guilabel:`Session type` that works can take some trial and error.
    Most MOAD workstations have the Mate window manager installed,
    but you may have to try others.
    Feel free to ask for help on the `SalishSeaCast #general`_ Slack channel,

To connect to ``chum``,
click the :guilabel:`chum` session tile on the right side of the X2Go window,
enter your password,
and click the :guilabel:`Okay` button.
If it is your first time connecting to ``chum`` you will get a :guilabel:`Host Key verification failed` alert that shows ``chum``'s public host key hash:
``85:41:ab:c4:a4:f3:7f:7b:c0:4c:8b:48:da:66:c9:5c:ec:8a:2b:69``;
click the :guilabel:`Yes` button to add ``chum``'s public key to your :file:`~/.ssh/known_hosts` file.

After some time,
and some logging messages appearing in a session status dialog in the main part of the X2Go window,
a new window showing ``chum``'s desktop should appear.

When you are finished with your desktop session,
close the remote desktop window,
and click the :guilabel:`Cancel` button in the session login dialog that replaces the session status dialog in the main part of the X2Go window.
