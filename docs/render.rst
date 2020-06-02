**Change image rendering settings and channel names using the Command Line Interface (CLI)**
============================================================================================
Description:
------------

This chapter will show how to change rendering settings on images using Command Line Interface (CLI).

This action is typically done after a successful import of images.

We will show:

-  How to change rendering settings of large amount of images on the CLI in a repeatable manner.

**Resources:**
--------------

-  Documentation:

   -  https://docs.openmicroscopy.org/latest/omero/users/cli/installation.html

   -  `https://docs.openmicroscopy.org/omero/latest/users/cli/index.html <https://docs.openmicroscopy.org/omero/latest/users/cli/index.html>`__

-  Data: example images from

   -  IDR data http://idr.openmicroscopy.org/webclient/?show=project-51

-  Rendering plugin for OMERO 

   - https://pypi.org/project/omero-cli-render/

-  Rendering yml files defining the various rendering parameters, such as the channel color, channel name and minimum and maximum values.

   - https://github.com/ome/training-scripts/blob/master/maintenance/preparation/renderingdef.yml
   - https://github.com/ome/training-scripts/blob/master/maintenance/preparation/renderingdef2.yml

-  Rendering mapping file has two columns, in the left-hand one there arethe images to be rendered and in the right-hand side column points to the appropriate ​renderingdef.yml​ for that image.

   - https://github.com/ome/training-scripts/blob/master/maintenance/preparation/renderingMapping.tsv

-  Shellscript for batch modification of rendering settings of images

   - https://github.com/ome/training-scripts/blob/master/maintenance/scripts/apply_rnd_settings_as.sh

Setup:
------

**Rendering plugin installation**

- Go to the environment where you installed your OMERO.cli as specified under -  https://docs.openmicroscopy.org/latest/omero/users/cli/installation.html

- Activate the virtual environment.

- Run
    
    ``$ pip install omero-cli-render``

**Step-by-step:**
-----------------

#.  On your local machine, open a terminal

#.  Activate the virtual environment where ``omero-py`` is installed or add it to ``PATH`` e.g.
    
    ``$ export PATH=/opt/omero/server/venv3/bin:$PATH``

#.  Change the rendering of images in one Dataset. Run
    
    ``$ ​omero render set Dataset:$ID local_path/to/renderingdef.yml``
    
    where the ​$ID​ is the ID of the ​CDK5RAP2-C​  Dataset.

#.  Verify the change in the browser.

#.  Change the rendering of images in two Datasets:a. Run  ​

    ``$ omero render set Dataset:$ID1 Dataset:$ID2 renderingdef2.yml``

#.  Modify the rendering settings in batch using a shellscript such as https://github.com/ome/training-scripts/blob/master/maintenance/scripts/apply_rnd_settings_as.sh which uses ​HQL ​to find the Images IDs in OMERO and deliver them to theomero-cli-render​ plugin. The script reads the Datasets in which the Images are locatedin OMERO are listed from a​ renderingMapping.tsv ​​file, such as https://github.com/ome/training-scripts/blob/master/maintenance/preparation/renderingMapping.tsv/ 

​#.  cd to the folder where the script is located.#.  Run the bash script with the default parameters

    ``$ ​sh apply_rnd_settings.sh``

    The script could be run by a facility manager on behalf of other users.