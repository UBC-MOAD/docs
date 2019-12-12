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
In your web browser, visit to the `file-manager app`_. In the file manager, click "--- start here, select a collection ---".

.. _file-manager app: https://app.globus.org/file-manager 

The collections corresponding to Compute Canada clusters are named:
* computecanada#graham-dtn
* computecanada#cedar-dtn
* computecanada#beluga-dtn

Let's say you want to transfer files from graham to beluga. In that case, you will first select the graham collection. You will be asked to login to your graham (compute canada) account, so that Globus can access your files. Then, your home directories will appear in the panel below the collection line. Within your directories, go to the files/folder you want to transfer and select them. 

To the right of the collection you are transferring files FROM, you can "--- select a collection ---" to transfer files TO. For this example, we would select the beluga collection (you'll find it by searching the name listed above). Your beluga directories should appear in the panel on the right and you can select the directory you want to transfer files to. While still on the panel on the right, click "Transfer or Sync to...". Then, the program will highlight the left panel and you hit the blue Start button at the bottom of the screen. Now your files should start transferring and you'll receive an email when they're done!
