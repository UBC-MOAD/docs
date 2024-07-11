.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _MOAD-VSCode:

*******
VS Code
*******

`VS Code`_ is a free,
open source,
cross-platform,
modern editor for code and text documents.
It is developed by Microsoft.

.. _VS Code: https://code.visualstudio.com/

It has Git support built in.
A large ecosystem of extensions provide support for many programming languages,
file formats,
and editing tasks.
Of particular use in the MOAD group are:

* the Python extension that includes support
  for conda environments and the ability to edit Jupyter notebooks directly in VS Code
* the Remote - SSH extension that enables you to use all of the features of VS Code
  on directories and files located on remote machines including the Waterhole machines,
  salish,
  and the Alliance HPC clusters.


Remote - SSH Extension Notes
============================

* If you use the Remote - SSH extension you should increase the connection timeout
  setting because ``salish`` and some of the Waterhole workstations take >30
  seconds to establish ssh connections.
  You can change that setting by using the menu to open the settings view:
  :guilabel:`File > Preferences > Settings` and using the search bar at the top of
  the view to find ``remote ssh connect timeout``.
  Changing the value from the default of 15 seconds to 45 seconds should ensure
  successful connections to ``salish`` and the Waterhole workstations that are slow
  to connect.

* Extensions that you want to use on the remote machine have to be installed have to
  be installed there.
  When you have a Remote - SSH session running,
  the Extensions sidebar shows extensions that you have installed locally in a
  :guilabel:`LOCAL - INSTALLED` list.
  If an extension can be installed for remote use a button like
  :guilabel:`Install in SSH: salish` should be available to trigger the remote installation.

  Extensions installed on the remote host show in a list with a name like
  :guilabel:`SSH: SALISH - INSTALLED`.

  So,
  to use Python and Jupyter in Remote - SSH connected to ``salish`` you will need to
  click the :guilabel:`Install in SSH: salish` in the Python extension tile in the Extensions
  sidebar.
  If you have the Python extension installed locally,
  you should be able to find its tile in the :guilabel:`LOCAL - INSTALLED` list.
  Otherwise, look for it in the :guilabel:`RECOMMENDED` list,
  or use the search box at the top of the Extension sidebar to find it.


Recommended Extensions
======================

All of these extensions can be installed from the Extensions sidebar.
The links included below are so that you can confirm that you are installing
the recommended one because there are many,
many extensions with very similar names.

Microsoft's Python Extension
    Includes Jupyter support and many other features

    https://marketplace.visualstudio.com/items?itemName=ms-python.python


Microsoft's Jupyter Notebook Renderers
    Renderers for various image/graph formats output in Jupyter notebooks

    https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter-renderers


Microsoft's Remote SSH Extension
    Uses :program:`ssh` to open any directory/folder on a remote machine so that
    its files can be edited in VS Code.

    Includes an editing mode for :program:`ssh` configuration files.

    https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh


GitLens
    Enhanced Git functionality and repository visualization

    https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens


Code Spell Checker
    A basic spell checker that works well with code and documents

    https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker


Clipboard
    A clipboard that remembers your last 200 copy/cut fragments

    https://marketplace.visualstudio.com/items?itemName=Digoro.Clipboard


Modern Fortran
    Syntax highlighting, etc. for Fortran

    https://marketplace.visualstudio.com/items?itemName=fortran-lang.linter-gfortran


Rainbow CSV
    Highlight in colour the columns of CSV and similar file types,
    and other helpers for working with those types of files

    https://marketplace.visualstudio.com/items?itemName=mechatroner.rainbow-csv


reStructuredText Syntax highlighting
    Syntax highlighting and document symbols for reStructuredText

    https://marketplace.visualstudio.com/items?itemName=trond-snekvik.simple-rst


vscode-pdf
    Display PDF files in VS Code

    https://marketplace.visualstudio.com/items?itemName=tomoki1207.pdf



.. _FortranLanguageServer:

Fortran Language Server (``fortls``)
====================================

The `fortls package <https://fortls.fortran-lang.org/index.html>`_ provides a lot of higher level,
IDE functionality to the Modern Fortran extension.
Among other things,
it does background compilation of files you are editing to provide in-editor compiler errors and
warnings.

To set up ``fortls`` in Remote - SSH sessions on `graham`,
`salish`,
etc.:

#. Use the Extensions sidebar to ensure that the Modern Fortran extension is enabled on the remote
   machine

#. In a terminal session on the remote machine,
   create a conda environment called ``fortls`` with the ``fortls`` package and its dependencies
   installed in it:

   .. code-block:: bash

    conda create -n fortls fortls

#. Confirm the path of that new ``fortls`` environment by looking at the output of
   :command:`conda env list`.
   You should see a line like:

   .. code-block:: text

      fortls                   /home/dlatorne/miniforge3/envs/fortls

   with your userid instead of Doug's.

#. In the VS Code settings,
   search for ``fortls``.
   Select the remote settings for the machine you are on;
   e.g. ``Remote [SSH: graham]``.
   Change the value of the ``Fortran > Fortls: Path`` setting to the ``fortls`` environment path
   with :file:`/bin/fortls` appended;
   e.g. :file:`/home/dlatorne/miniforge3/envs/fortls/bin/fortls`

The language server compiles the code in the files you are editing to provide you with compiler
errors and warnings and other features as you type.
That results in the creation of ``.mod`` files beside your Fortran source code files;
i.e. if you are editing :file:`bdydta.F90`,
a :file:`bdydta.mod` will be created.
``.mod`` files are compiler product files that *should not* be committed to version control.
Please just ignore them!

For namelist files you may have to explicitly tell VS Code to use the Fortran language mode
because our naming convention is different to what the extension and language server expect.
Near the right side of the bottom bar of the editor,
find the language mode selector.
For namelist it will probably say ``Plain Text``,
or occasionally ``HSLS``.
Click the selector and choose ``Fortran (FortranFreeForm)``.
