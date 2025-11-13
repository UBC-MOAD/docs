.. Copyright 2018 – present by The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   https://creativecommons.org/licenses/by/4.0/


.. _Globus-docs:

********************
Globus File Transfer
********************

`Globus`_ does high-performance data transfers between systems within an organization
(in our usage between Alliance clusters).
Transfer rates vary between 200-400 MB/s; a 3.6 TB transfer took about 3 hours.

.. _Globus: https://www.globus.org/data-transfer

First step: create an account on the globus website and follow the login instructions.


Transferring Files
------------------

In your web browser, go to the `file-manager app`_ .
Note that the collections corresponding to Compute Canada clusters are named:

* Fir  --- alliancecan#fir-globus
* Nibi --- alliancecan#nibi
* Naval --- Compute Canada - Narval
* Rorqual --- alliancecan#rorqual
* Trillium --- alliancecan#trillium

.. _file-manager app: https://app.globus.org/file-manager

As an example, let's say you want to transfer files from ``nibi`` to ``fir``.
These are the steps you would follow:

#. In the file manager, click "--- start here, select a collection ---" and search for the Nibi collection
   (names listed above).

#. You will be asked to login to your Alliance account.
   Login and your home directories will appear in the panel below the collection line.

#. Within your directories, go to the files/folder you want to transfer and select them.

#. To the right of the collection you are transferring files FROM,
   "--- select a collection ---" to transfer files TO.
   In this case,
   select the ``fir`` collection

#. Your ``fir`` directories should appear in the panel on the right.
   Select the directory you want to transfer files to.

#. While still on the panel on the right,
   click "Transfer or Sync to..." and the program will highlight the left panel.

#. Press the blue Start button at the bottom of the screen.

Your files will start transferring and you'll receive an email when they're done!
