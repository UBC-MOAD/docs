.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _DocumentationWithSphinx:

*************************
Documentation with Sphinx
*************************

We use `Sphinx`_ for most documentation in the UBC-MOAD projects and repositories.
`Sphinx`_ provides:

* direct rendering to HTML for online publication
* easy inclusion of `LaTeX`_ math syntax with in-browser rendering via `MathJax`_
* easy inclusion of figures, graphs, and images
* easy inclusion of Jupyter Notebooks
* deep linkability
* high search engine discoverability for docs that we publish to `readthedocs`_
* optional PDF rendering

.. _Sphinx: https://www.sphinx-doc.org/en/master/
.. _LaTeX: https://www.latex-project.org/
.. _MathJax: https://www.mathjax.org/
.. _readthedocs: https://readthedocs.org/

`LaTeX`_ should be used for manuscripts of papers and theses,
and for reports for which PDFs must be rendered,
uploaded,
and linked into other documentation to make them available online.

All documentation is under :ref:`vc-with-git` and stored in either docs repositories
(e.g. `UBC-MOAD/docs`_,
`SalishSeaCast/docs`_ ,
and `MIDOSS/docs`_)
or in the :file:`docs/` directory of :ref:`code repositories <team-repos>`
(e.g. `UBC-MOAD/moad_tools`_,
or `SalishSeaCast/NEMO-Cmd`_).

.. _UBC-MOAD/docs: https://github.com/UBC-MOAD/docs
.. _SalishSeaCast/docs: https://github.com/SalishSeaCast/docs
.. _MIDOSS/docs: https://github.com/MIDOSS/docs
.. _UBC-MOAD/moad_tools: https://github.com/UBC-MOAD/moad_tools
.. _SalishSeaCast/NEMO-Cmd: https://github.com/SalishSeaCast/NEMO-Cmd

Most of our repositories are configured so that when changes that have been committed and  pushed to GitHub a signal is sent to `readthedocs.org`_ to automatically rebuild and render the docs at a :kbd:`readthedocs.io` sub-domain.
There is a very good chance that you are presently reading the result of that processing pipeline at https://ubc-moad-docs.readthedocs.io/en/latest/sphinx_docs.html#documentation-with-sphinx.

.. _readthedocs.org: https://readthedocs.org/

Links to a repository's docs can generally be found in the :file:`README`.

Sphinx_ uses reStructuredText
(reST),
a simple,
unobtrusive markup language.
The `Sphinx documentation`_ provides a brief `introduction to reST concepts and syntax`_.
Sphinx extends reST with a `collection of directives and interpreted text roles`_ for
cross-referencing,
tables of contents,
code examples,
and specially formatted paragraphs like
notes,
alerts,
warnings,
etc.

.. _Sphinx documentation: https://www.sphinx-doc.org/en/master/
.. _introduction to reST concepts and syntax: https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html
.. _collection of directives and interpreted text roles: https://www.sphinx-doc.org/en/master/usage/restructuredtext/index.html


Installing Sphinx
=================

To use Sphinx you need to have the :kbd:`sphinx` and :kbd:`sphinx_rtd_theme` packages installed in your :program:`conda` environment.
If you want to include Jupyter Notebooks in your docs you also need the :kbd:`nbsphinx` package.

Our docs and code repositories docs all have a section about contributing or package development that includes details of how to setup up a :program:`conda` environment to work in,
and how to build,
preview,
and link-check the repository's docs.
For this repository please see :ref:`MOAD-DocsContributing`.
:ref:`moadtools:moad_toolsPackagedDevelopment` is an example of a code repository package development docs sections that includes information about working on its docs.


.. _BuildingAndPreviewingDocumentation:

Building and Previewing Documentation
=====================================

As you are writing and editing Sphinx documentation you can build the HTML rendered docs locally and preview them in your browser to ensure that there are no reST syntax errors and that the docs look the way you want them to.

In the top level :file:`docs/` directory
(e.g. :file:`docs/` in this repository,
or the :file:`docs/` sub-directory in a code repository)
use the command:

.. code-block:: bash

    make clean html

to build the docs.
You will be notified of any syntax or consistency errors.

The HTML pages produced by the :command:`make clean html` command are stored in the :file:`_build/html/` sub-directory.
You can use your browser to open the :file:`index.html` file in that directory to preview them.
The command:

.. code-block:: bash

    firefox _build/html/index.html

will probably do the right thing.
You can keep a browser tab open to the rendered docs and refresh after each build to see updates.

.. note::

    The top level :file:`docs/` directory contains
    (at minimum)
    the files
    :file:`conf.py`,
    :file:`Makefile`,
    and :file:`index.rst`,
    and the directory :file:`_static/`.
    After the docs have been built it will also contain the :file:`_build/` sub-directory.

The result of running :command:`make clean html` should look something like::

  Removing everything under '_build'...
  Running Sphinx v3.5.2
  making output directory... done
  loading intersphinx inventory from https://ubc-moad-tools.readthedocs.io/en/latest/objects.inv...
  loading intersphinx inventory from https://nemo-cmd.readthedocs.io/en/latest/objects.inv...
  loading intersphinx inventory from https://salishseacmd.readthedocs.io/en/latest/objects.inv...
  loading intersphinx inventory from https://salishsea-meopar-docs.readthedocs.io/en/latest/objects.inv...
  building [mo]: targets for 0 po files that are out of date
  building [html]: targets for 20 source files that are out of date
  updating environment: [new config] 20 added, 0 changed, 0 removed
  reading sources... [100%] zzz_archival_docs/index
  looking for now-outdated files... none found
  pickling environment... done
  checking consistency... done
  preparing documents... done
  writing output... [100%] zzz_archival_docs/index
  generating indices... done
  writing additional pages... search done
  copying static files... done
  copying extra files... done
  dumping search index in English (code: en)... done
  dumping object inventory... done
  build succeeded.

  The HTML pages are in _build/html.


.. _LinkCheckingDocumentation:

Link Checking the Documentation
===============================

You can also check the documentation for broken links with the command:

.. code-block:: bash

    make clean linkcheck

Look for any errors in the output or in the :file:`_build/linkcheck/output.txt` file.


Writing Style
=============

Please consider using `semantic line breaks`_ in your Sphinx files.
Doing so makes it easier to quickly rearrange clauses and ideas as you edit and revise.
It also makes it *so* much easier to see changes in context when you use :command:`git diff` or look at commits on GitHub.

.. _semantic line breaks: https://rhodesmill.org/brandon/2012/one-sentence-per-line/


Links and Cross-references
==========================

.. _SphinxExternalLinks:

External Links
--------------

The preferred way to including external links is via markup like::

  This is a paragraph that contains `a link`_.

  .. _a link: http://example.com/

If the link text should be the web address,
you don't need special markup at all,
the parser finds links and mail addresses in ordinary text.


Internal Links
--------------

To support cross-referencing to arbitrary locations in any document,
the standard reST labels are used.
For this to work label names must be unique throughout the entire documentation.
There are three ways in which you can refer to labels:

#. If you place a label directly before a section title,
   you can reference to it with ``:ref:`label-name```.
   Example::

     .. _my-reference-label:

     Section to cross-reference
     --------------------------

     This is the text of the section.

     It refers to the section itself, see :ref:`my-reference-label`.

   The ``:ref:`` role would then generate a link to the section,
   with the link title being "Section to cross-reference".
   This works just as well when sections and references are in different source files.

   Labels also work with figures.
   Given::

     .. _my-figure:

     .. figure:: whatever

        Figure caption

   a reference ``:ref:`my-figure``` would insert a reference to the figure
   with link text "Figure caption".

   The same works for tables that are given an explicit caption using the
   :kbd:`table` directive.

#. Labels that aren't placed before a section title can still be referenced to,
   but you must give the link an explicit title,
   using this syntax: ``:ref:`Link title <label-name>```.

   The same syntax can be used to change the link text from what it would be automatically to something different that you want in a specific context.
   Example::

     :ref:`the section above <my-reference-label>`

   makes a link to the :kbd:`Section to cross-reference` section with :kbd:`the section above` as the link text.

#. The `intersphinx`_ extension automatically generates links to labels and objects in Sphinx docs in other repositories.
   Example::

     :ref:`moadtools:moad_toolsPackagedDevelopment`

   creates a link to the :ref:`moadtools:moad_toolsPackagedDevelopment` section in the `UBC-MOAD/moad_tools`_ docs.

   .. _intersphinx: https://www.sphinx-doc.org/en/master/usage/extensions/intersphinx.html#module-sphinx.ext.intersphinx

Using :rst:role:`ref` is advised over the :ref:`SphinxExternalLinks` style whenever possible because it works across files,
and when section headings are changed.


Links to Rendered Jupyter Notebooks
-----------------------------------

To link to a rendered representation of an Jupyter Notebook that has been pushed to a GitHub repo use markup like::

  * `SalishSeaBathy.ipynb`_: Documents the full domain bathymetry used for the Salish Sea NEMO runs.

  .. _SalishSeaBathy.ipynb: https://nbviewer.org/github/SalishSeaCast/tools/blob/master/bathymetry/SalishSeaBathy.ipynb


Forcing Line Breaks
===================

In most cases your should just let Sphinx take care of inserting line breaks in the rendered docs;
it will almost always do the right thing by putting breaks between paragraphs,
between list items,
around block quotations and code examples,
etc.

Occasionally though you may need to force line breaks.
The most common case for this is to add line breaks within table cells so as as to avoid excessive sideways scrolling of the rendered table.
You can force a line break in the HTML that Sphinx renders by defining a substitution that will insert a break tag (:kbd:`<br>`).
Here's an example of doing that and using the substitution in a table cell::

  .. |br| raw:: html

      <br>

  ===========  ===================================================  ==============  ==================
   Date                       Change                                New Value       Changeset
  ===========  ===================================================  ==============  ==================
  27-Oct-2014  1st :file:`nowcast/` run results                     N/A
  20-Nov-2014  1st :file:`forecast/` run results                    N/A
  26-Nov-2014  Changed to tidal forcing tuned for better |br|       see changeset   efa8c39a9a7c_
               accuracy at Point Atkinson
  ===========  ===================================================  ==============  ==================

.. note:: The :kbd:`|br|` substitution needs to be defined once (but *only* once) per file.
