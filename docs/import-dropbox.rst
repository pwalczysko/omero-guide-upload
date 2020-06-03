Import data using OMERO.dropbox
===============================

**Description:**
----------------

OMERO.dropbox allows to import files into OMERO automatically, by means
of offline import from a watched directory. Typically, each user has a
folder into which they "drop" their images as these are being acquired.
The folder can be directly on the machine where OMERO is installed, or,
more commonly, OMERO can watch directories mounted on the machine where OMERO is installed.

Alternatively, the files to import can be copied into the folders watched by OMERO.dropbox by cron
jobs \ https://en.wikipedia.org/wiki/Cron\  or systems developed by the community users such as \ https://github.com/imcf/auto-tx\ , which automatically harvest the files from acquisition computers and transfer them onto shared network drives.

We will show

-  How to set up OMERO.dropbox on your OMERO.server.

-  How to set up the directories/folders into which the users or the automatic systems described above will copy the files to import.

-  How to import several files for two different users using OMERO.dropbox. The image files are manually copied into the prepared folders watched by OMERO.dropbox.

**Resources:**
--------------

-  Documentation:

   -  https://docs.openmicroscopy.org/latest/omero/sysadmins/dropbox.html

   -  https://docs.openmicroscopy.org/omero/latest/users/cli/import-target.html

-  Data: example images from \ https://downloads.openmicroscopy.org/images/DV/alexia/cajal-bodies/

**Setup:**
----------

This example setup will help you to understand OMERO.dropbox functionality. Later, you can choose the setup you like for your OMERO.server studying detailed instructions at \ https://docs.openmicroscopy.org/latest/omero/sysadmins/dropbox.html#advanced-use

Below are the installation instructions.

-  First connect to the OMERO.server over SSH using your administrator account.

-  In the OMERO.server directory set the following lines to enable and configure Dropbox:

   - ``$ bin/omero config set omero.fs.watchDir "/home/DropBox"``

   - ``$ bin/omero config set omero.fs.importArgs "-T \\"regex:^.*/(?<Container1>.*?)\""``

..

   Note: The last line makes sure the created Dataset into which the images will be imported will have the same name as the deepest folder
   in the DropBox folder for that particular user. Every image in any Experiment-1 directory will end up in the same Dataset for that user, however the superstructure of folders have been.

   To set up different import options, go
   to \ https://docs.openmicroscopy.org/omero/latest/users/cli/import-target.html\ .

   For example, it is possible to create a Dataset with a fixed name for every user or to create a Dataset whose name is picked from the first subfolder under images folder.

-  On the OMERO.server machine, create a folder called DropBox under /home.

-  ``$ cd /home``

-  ``$ mkdir DropBox``

-  Create under the ``/home/DropBox`` directory two subdirectories for two users e.g. user-1 and user-2. The names of these directories should match the login names of those users in OMERO.

   Note: the omero system user must be able to read from those directories. Also, the users or the system which will drop files into those directories must have write permissions there.


-  ``$ cd /home/DropBox``

-  ``$ mkdir user-1``

-  ``$ mkdir user-2``

Step-by-step:
-------------

#. Open a browser window.

#. Enter the URL provided.

#. Login as user-1 or any user with the right to see user-1’s data.

#. Leave the browser window open.

#. On your computer, open a new terminal window.

#. Connect over SSH to the machine where the OMERO.server is running.

#. Open the logfile DropBox.log under var/log/ e.g.

   -  ``$ tail -f /path/to/OMERO.server/var/log/DropBox.log``

#.  Open a new terminal window.

#.  Connect over SSH to the machine where the OMERO.server is running.

#. In any folder on that machine, create a directory Experiment-1 into which you copy an image. Drop the Experiment-1 directory into the user-1 folder you created during the setup above:

   -  ``$ cd /path/to/other/dir``

   -  ``$ mkdir Experiment-1``

   -  ``$ scp 090829_5_HeLa_siCTL_coilin_ATUB01_05_R3D_D3D.dv ./Experiment-1``

   -  ``$ cd /home/DropBox``

   -  ``$ scp -r /path/to/other/dir/Experiment-1 ./user-1`` #copy the whole directory “Experiment-1” into the directory watched by OMERO

#. The OMERO.dropbox will intentionally wait for 60 seconds between registration of the new drop into the folder and the actual import, in anticipation of further file drops of files into the DropBox folder.

#. After you have copied the directory into the user-1 folder, you should see in the DropBox.log lines like as follows.

.. code-block:: python

   2019-08-12 17:09:23,644 INFO [ fsclient.fsDropBoxMonitorClient]
   (Thread-3 ) New entry
   /home/DropBox/user-1/Experiment-1/090829_5_HeLa_siCTL_coilin_ATUB01_05_R3D_D3D.dv
   contains 1 file(s). Files=1 Timers=1

   2019-08-12 17:10:23,644 INFO [ fsclient.fsDropBoxMonitorClient]
   (Thread-17 ) Removed key
   /home/DropBox/user-1/Experiment-1/090829_5_HeLa_siCTL_coilin_ATUB01_05_R3D_D3D.dv

   2019-08-12 17:10:23,666 INFO [ fsclient.fsDropBoxMonitorClient]
   (Thread-17 ) Importing
   /home/DropBox/user-1/Experiment-1/090829_5_HeLa_siCTL_coilin_ATUB01_05_R3D_D3D.dv
   (session=2155e6d0-445a-496b-a6bc-8afeb93ac58d)

   2019-08-12 17:10:23,955 INFO [ omero.util.Resources] (Thread-18 )
   Starting

   2019-08-12 17:10:23,961 WARNI [ stderr] (Thread-17 ) Joined session
   for user-2@localhost:4064. Idle timeout: 10 min. Current group: Lab1

   2019-08-12 17:10:31,989 INFO [ fsclient.fsDropBoxMonitorClient]
   (Thread-17 ) Import of
   /home/DropBox/user-1/Experiment-1/090829_5_HeLa_siCTL_coilin_ATUB01_05_R3D_D3D.dv
   completed (session=2155e6d0-445a-496b-a6bc-8afeb93ac58d)

   2019-08-12 17:10:32,001 INFO [ omero.util.Resources] (Thread-18 )
   Halted


#. Go back to OMERO.web. Refresh the tree.

#. Observe that a new Dataset was created, with the name Experiment-1. The image is imported into that Dataset.

#. The image is always imported into the default group of the user.

#. Repeat the workflow for user-2. First, go to your browser, logout and login again as user-2.

#. Connect over SSH to the machine where the OMERO.server is running if required.

#. Create again a folder, Experiment-2.

#. Copy an image into it

#. Copy the whole Experiment-2 folder under /home/DropBox/user-2.

#. Go back to the browser, refresh and verify that you can see a Dataset Experiment-2 under user-2’s data with the image inside.

#. Note: Even if user-2’s folder in the previous workflow uses the same name for their dataset as user-1 (Experiment-1), the data of user-2 would not be imported into user-1’s Dataset. Instead, a new Dataset Experiment-1 would be created under user-2’s data, belonging to user-2, into which the image would be imported.
