.. _graphics:

===========
Graphics
===========

.. note::
   Graphics plotting is not a supported feature for the MRW release. The instructins provided here are for informational purposes only. Users may experiment with the scripts at their own risk. 

The MRW Application comes with two Python scripts for users to visually check the results of their forecast experiments. This script plots 2-m temperature, hourly precipitation, cloud cover, and 10-m wind at a user inputted time range. In addition, it creates a forecast loop GIF for each forecast product.

Users will need to load Anaconda first. Here are some examples of how to do this:

..
   COMMENT: Is this actually miniconda now?

On an HPC system, you can use modules to load Anaconda. Otherwise, use the following instructions. Find download link here: https://www.anaconda.com/products/individual. Scroll to the bottom to the "Anaconda Installers" section and copy the download link for the appropriate version:

.. code-block:: console

   cd $HOME
   wget <download_link>
   bash <Anaconda_excecutable_name>
   export PATH=$HOME/anaconda3/bin:$PATH

Install Libraries:

.. code-block:: console

   conda install -c conda-forge -y cartopy
   conda install -y netCDF4

Download Files: 

   cd $SCRATCH

Get natural earth files

.. code-block:: console

   wget https://ftp.emc.ncep.noaa.gov/EIB/UFS/SRW/v1p0/natural_earth/natural_earth_ufs-srw-release-v1.0.0.tar.gz
   tar -xzf natural_earth_ufs-srw-release-v1.0.0.tar.gz
   wget <TODO_new_URL>

Run Script: 
Navigate to the run directory of your experiment

.. code-block:: console

   cd $SCRATCH/<expt_name>/run

Create symbolic link to plotting script:

.. code-block:: console
   
   ln -sf dir/of/plot_mrw.py .

Run script:

.. code-block:: console

   python3 plot_mrw.py (Start time)<YYYYMMDDHH> (Start Forecast hour)<HHH> (End Forecast hour)<HHH> <Natural Earth Directory>

For example:

.. code-block:: console

   python3 plot_mrw.py 2019082900 000 048 $SCRATCH/natural_earth


View Results:
A utility typically used to visualize the resulting images in png format is display. If it is available on your platform, you can use the command:

.. code-block:: console

   display *.png

Users working on a remote platform can use the ``scp`` command to transfer files to their local system to view the GIFs and files.


Sample Output
The sample plots are in the ``sample_output.pdf`` document in the ``plotting_scripts`` directory. They are consistent with the Hurricane Dorian initial conditions and tag ufs-v1.1.0. Users results may look different if they are using a different branch or tag. Their results may also look different if they are running on a different platform than the one used to generate the plots.

..
   COMMENT: Update paragraph above. Esp: Hurricane Dorian? tag#

Additional Options
After completing the graphics plotting successfully, users may be interested in trying to change a namelist option and run a second test to check their understanding of how results will change. If you are interested in doing that, please visit the UFS Portal to take our graduate student test, which will give you instructions to take that leap, and will also provide important information for our development work

..
   COMMENT: Link to GST?




Plot Results from MRW Graduate Student Test (GST)
====================================================



``plot_mrw_cloud_diff.py``
------------------------------

The ``plot_mrw_cloud_diff.py`` generates plots of cloud cover from the UFS MRW App Graduate Student Test (GST). Users plot a control, experiment, and the difference between the control and the experiment.

Make sure all the necessary modules can be imported.

..
   COMMENT: Which modules are these? Same as for SRW?

Five command line arguments are required:
   #. Cycle date/time in YYYYMMDDHH format
   #. Forecast hour in HHH format
   #. ``CTRL_DIR``: Control directory. Postprocessed data should be found in the directory: ``CTRL_DIR/run/``
   #. ``EXPT_DIR``: Experiment directory. Postprocessed data should be found in the directory: ``EXPT_DIR/run/``
   #. ``CARTOPY_DIR``: Base directory of cartopy shapefiles. Shapefiles cannot be directly downloaded to NOAA machines from the internet, so shapefiles need to be downloaded if geopolitical boundaries are desired on the maps. The file structure should be: ``CARTOPY_DIR/shapefiles/natural_earth/cultural/*.shp``. For more information regarding files needed to set up display maps in Cartopy, see the `SRW App Users' Guide <https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.0/>`__. 

Plots are saved in the ``plotting_scripts`` as ``YYYYMMDDHH<variable>_<domain>_fHHH.png``. The variable domains in this script can be set to either 'conus' for a :term:`CONUS` map or manually adjusted in the Domain section


plot_mrw.py
--------------

This script generates plots of several output variables from the UFS MRW App Graduate Student Test. 

Make sure all the necessary modules can be imported. Five command line arguments are needed:
#. Cycle date/time in YYYYMMDDHH format
#. Start forecast hour in HHH format
#. End forecast hour in HHH format
#. ``CARTOPY_DIR``:  Base directory of cartopy shapefiles. Shapefiles cannot be directly downloaded to NOAA machines from the internet, so shapefiles need to be downloaded if geopolitical boundaries are desired on the maps. The file structure should be: ``CARTOPY_DIR/shapefiles/natural_earth/cultural/*.shp``. For more information regarding files needed to setup display maps in Cartopy, see the `SRW App Users' Guide <https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.0/>`__. 

..
   COMMENT: Says five arguments are needed but only 4 are listed...
   COMMENT: Update SRW App link here and above. 


Plots are saved in the ``plotting_scripts`` as ``YYYYMMDDHH<variable>_<domain>_fHHH.png``. The variable domains in this script can be set to either 'conus' for a CONUS map or manually adjusted in the Domain section. 

..
   COMMENT: What are the "Domain" section options? 


