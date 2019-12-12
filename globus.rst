.. Copyright 2018-2019 The UBC EOAS MOAD Group
.. and The University of British Columbia
..
.. Licensed under a Creative Commons Attribution 4.0 International License
..
..   http://creativecommons.org/licenses/by/4.0/


.. _Globus-docs:

******
Globus file transfer
******

`Globus`_ does high-performance data transfers between systems within an organization (in our usage between Compute Canada clusters). Transfer rates vary between 200-400 MB/s; a 3.6 TB transfer took about 3 hours.

.. _Globus: https://www.globus.org/data-transfer

First step: create an account on the globus website and follow the login instructions. 


Transferring files
---------------------------
In your web browser, go to the `file-manager app`_ . Note that the collections corresponding to Compute Canada clusters are named:

* Graham --- computecanada#graham-dtn 
* Cedar  --- computecanada#cedar-dtn 
* Beluga --- computecanada#beluga-dtn 

.. _file-manager app: https://app.globus.org/file-manager 

As an example, let's say you want to transfer files from graham to beluga. These are the steps you would follow:

#. In the file manager, click "--- start here, select a collection ---" and search for the Graham collection (names listed above). 

#. You will be asked to login to your compute canada account. Login and your home directories will appear in the panel below the collection line. 

#. Within your directories, go to the files/folder you want to transfer and select them.

#. To the right of the collection you are transferring files FROM, "--- select a collection ---" to transfer files TO. In this case, select the beluga collection

#. Your beluga directories should appear in the panel on the right. Select the directory you want to transfer files to. 

#. While still on the panel on the right, click "Transfer or Sync to..." and the program will highlight the left panel.

#. Press the blue Start button at the bottom of the screen. 
Your files will start transferring and you'll receive an email when they're done!
