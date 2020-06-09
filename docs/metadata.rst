Import metadata using the Command Line Interface (CLI)
======================================================

Description:
------------

This chapter will show how to import metadata starting from a local CSV file and ending with OMERO.tables on images or Key-Value pairs on images using the Command Line Interface (CLI) and the OMERO server-side scripts.

This action is typically done after a successful import of images.

We will show:

- How to import metadata from local CSV file in a bulk manner and turn them into OMERO.tables on images using CLI

- How to turn the OMERO.tables on images into Key-Value pair annotations on images in bulk manner using CLI

- How to import metadata from local CSV file and use a server-side script in OMERO to turn these into OMERO.tables on images

- How to construct a simple file to turn the metadata stored in OMERO.tables into Key-Value pairs on images using CLI

**Resources:**
--------------

-  Documentation:

   -  https://docs.openmicroscopy.org/omero/latest/users/cli/installation.html

   -  `https://docs.openmicroscopy.org/omero/latest/users/cli/index.html <https://docs.openmicroscopy.org/omero/latest/users/cli/index.html>`__

-  Data: example images from

   -  IDR data http://idr.openmicroscopy.org/webclient/?show=project-51

-  Metadata plugin for OMERO

   - https://pypi.org/project/omero-metadata/

-  Bulkmap config yml files defining the various Key-Value pairs parameters, such as the groups and other parameters.

   - https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-bulkmap-config.yml
   - :download:`simple-annotation-bulkmap-config.yml <simple-annotation-bulkmap-config.yml>`

-  Annotation csv files define the content of OMERO.tables for each image.

   - https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-annotation.csv
   - :download:`simple-annotation.csv <simple-annotation.csv>`
   - :download:`four-images.csv <four-images.csv>`

Setup:
------

**Metadata plugin installation**

- Go to the environment where you installed your OMERO.cli as specified under -  https://docs.openmicroscopy.org/omero/latest/users/cli/installation.html.

- Activate the virtual environment.

- Run::
    
    $ pip install omero-metadata

**Step-by-step:**
-----------------

#.  On your local machine, open a terminal

#.  Activate the virtual environment where ``omero-py`` is installed or add it to ``PATH`` e.g.::

    $ export PATH=/opt/omero/server/venv3/bin:$PATH

#.  Download the csv from https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-annotation.csv 

#.  The variable ``$ID​`` below is the ID of the ​Project, in this example case it is the Project containing the idr0021 study. To add annotations from a local csv file to the images in the said Project in the form of OMERO.tables, run::
    
    $ omero metadata populate --report --batch 1000 --file local/path/to/idr0021-experimentA-annotation.csv Project:$ID

#.  Open your browser and login to the OMERO.web. Navigate to the Project you just worked with, expand the "Attachments" harmonica in the right-hand pane and verify that a new attachment is on that Project named ``bulk_annotations``.

    |image0|

#.  You can inspect its content by clicking on the "eye" icon |image1| inside the annotation.

#.  Select an image inside the Project and expand the "Tables" harmonica in the right-hand pane. These tables contain the appropriate line from the ``bulk_annotations`` attachment you just created for that particular image.

    |image2|

#.  Go back to your terminal. Download the idr0021-experimentA-bulkmap-config.yml file https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-bulkmap-config.yml.

#.  Open the downloaded file in a text editor and delete the ``Advanced options...`` section. Save the file and run::

    $ omero metadata populate --context bulkmap --cfg local/path/to/idr0021-experimentA-bulkmap-config.yml --batch 100 Project:$ID

#.  Go to your browser and OMERO.web, select the images in the Project you targeted and verify that they have now new Key-Value pair annotations displayed in the right-hand pane.

#.  Still in OMERO.web, create a new Dataset and copy into it 4 images, preferably images which have neither OMERO.tables on them nor any Key-Value pairs attached. Note the names of the images you are copying in.

    |image3|

#.  Go to the https://pypi.org/project/omero-metadata/ and find the section named ``populate``. Study the ``project.csv``. You can either take the ``project.csv`` file from there, or more conveniently, you can download directly its copy :download:`four-images.csv <four-images.csv>`. Open the csv in Excel and edit the names of the images in the first column to match the names of the images you copied into your Dataset in the previous step. Also, edit the name of the Dataset in the second column to match the name of your Dataset in OMERO.web. Save the file locally as csv.

#.  In your OMERO.web, upload the csv you just saved and attach it onto the Dataset you created previously.

    |image4|

#.  Select the Dataset you created and attached to it the csv. Find the script icon |image5| above the central pane, expand it and find the ``Import scripts`` section. In there, select the ``Populate metadata`` script.

    |image6| 

#.  Run the script.

#.  Click again onto the Dataset in the left-hand pane to refresh and observe that there is a new Attachment in the right hand pane under "Attachments" harmonica. 

    |image7|

#.  Click on single images inside the Dataset and observe that in the "Tables" harmonica in the right-hand pane there are new values coming originally from your edited csv.

    |image8|


.. |image0| image:: images/metadata1.png
   :width: 4in
   :height: 1in

.. |image1| image:: images/metadata2.png
   :width: 0.35in
   :height: 0.3in

.. |image2| image:: images/metadata3.png
   :width: 4in
   :height: 3in

.. |image3| image:: images/metadata4.png
   :width: 5in
   :height: 1.5in

.. |image4| image:: images/metadata5.png
   :width: 4in
   :height: 1in

.. |image5| image:: images/metadata6.png
   :width: 0.35in
   :height: 0.3in

.. |image6| image:: images/metadata7.png
   :width: 2in
   :height: 0.7in

.. |image7| image:: images/metadata8.png
   :width: 4in
   :height: 1.3in

.. |image8| image:: images/metadata9.png
   :width: 4in
   :height: 2in
