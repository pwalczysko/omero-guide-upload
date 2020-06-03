Import metadata using the Command Line Interface (CLI)
=======================================================

Description:
------------

This chapter will show how to import metadata starting from a local CSV file and ending with OMERO.tables on images or Key-Value pairs on images using the Command Line Interface (CLI) and the OMERO server-side scripts.

This action is typically done after a successful import of images.

We will show:

-  How to import metadata from local CSV file in a bulk manner and turn them into OMERO.tables on images using CLI

- How to turn the OMERO.tables on images into Key-Value pair annotations on images in bulk manner using CLI

- How to import metadata from local CSV file and use a server-side script in OMERO to turn these into OMERO.tables on images

- How to construct a simple file to turn the metadata stored in OMERO.tables into Key-Value pairs on images using CLI

**Resources:**
--------------

-  Documentation:

   -  https://docs.openmicroscopy.org/latest/omero/users/cli/installation.html

   -  `https://docs.openmicroscopy.org/omero/latest/users/cli/index.html <https://docs.openmicroscopy.org/omero/latest/users/cli/index.html>`__

-  Data: example images from

   -  IDR data http://idr.openmicroscopy.org/webclient/?show=project-51

-  Metadata plugin for OMERO

   - https://pypi.org/project/omero-metadata/

-  Bulkmap config yml files defining the various Key-Value pairs parameters, such as the groups and other parameters.

   - https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-bulkmap-config.yml

-  Annotation csv file defines the content of OMERO.tables for each image.

   - https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-annotation.csv

Setup:
------

**Metadata plugin installation**

- Go to the environment where you installed your OMERO.cli as specified under -  https://docs.openmicroscopy.org/latest/omero/users/cli/installation.html.

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

#.  You can inspect its content by clicking on the "eye" icon inside the annotation.

#.  Select an image inside the Project and expand the "Tables" harmonica in the right-hand pane. These tables contain the appropriate line from the ``bulk_annotations`` attachment you just created for that particular image.

#.  Go back to your terminal. Download the idr0021-experimentA-bulkmap-config.yml file https://github.com/IDR/idr0021-lawo-pericentriolarmaterial/blob/9479af85f19487f215e3dfdd31a1b587370ed3cf/experimentA/idr0021-experimentA-bulkmap-config.yml.

#.  Open the downloaded file in a text editor and delete the ``Advanced options...`` section. Save the file and run::

    $ omero metadata populate --context bulkmap --cfg local/path/to/idr0021-experimentA-bulkmap-config.yml --batch 100 Project:$ID

#.  Go to your browser and OMERO.web, select the images in the Project you targeted and verify that they have now new Key-Value pair annotations displayed in the right-hand pane.

#.  Still in OMERO.web, create a new Dataset and copy into it 4 images, preferably images which have nor OMERO.tables on them neither any Key-Value pairs attached. Note the names of the images you are copying in.

#.  Go to the https://pypi.org/project/omero-metadata/ and find the section named ``populate``. Copy the ``project.csv`` from that section and paste it locally, either into your text editor, or into Microsoft Excel. Edit the names of the images in the first column to match the names of the images you copied into your Dataset in the previous step. Also, edit the name of the Dataset in the second column to match the name of your Dataset in OMERO.web. Save the file locally as csv.

#.  In your OMERO.web, upload the csv you just saved and attach it onto the Dataset you created previously.

#.  Select the Dataset you created and attached to it the csv. Find the script icon above the central pane, expand it and find the ``Import scripts`` section. In there, select the ``Populate metadata`` script.

#.  Run the script.

#.  Go back to the Dataset and observe that there is a new Attachment in the right hand pane under "Attachments" harmonica. Click on single images inside the Dataset and observe that in the "Tables" harmonica in the right-hand pane there are new values coming originally from your edited csv.