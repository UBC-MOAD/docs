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
In your web browser, go to the `file-manager app`_. In the file manager, click "--- start here, select a collection ---"

.. _file-manager app: https://app.globus.org/file-manager 

The collections corresponding to Compute Canada clusters are named:
- computecanada#graham-dtn
- computecanada#cedar-dtn
- computecanada#beluga-dtn

As an example, let's say you want to transfer files from graham to beluga. These are the steps you would follow:
1. In the file manager, click "--- start here, select a collection ---" and search for the Graham collection (names listed above). 
2. You will be asked to login to your graham (compute canada) account. Login and your home directories will appear in the panel below the collection line. 
3. Within your directories, go to the files/folder you want to transfer and select them.
4. To the right of the collection you are transferring files FROM, "--- select a collection ---" to transfer files TO. For this example, select the beluga collection (name listed above)
5. Your beluga directories should appear in the panel on the right. Select the directory you want to transfer files to. 
6. While still on the panel on the right, click "Transfer or Sync to..." and the program will highlight the left panel.
7. Press the blue Start button at the bottom of the screen. Your files will start transferring and you'll receive an email when they're done!
