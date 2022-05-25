.. _graphics:

===========
Graphics
===========

For visually checking the results of your run, we have provided a Python script that plots 2-m temperature, hourly precipitation, cloud cover, and 10-m wind at a user inputted time range. In addition, a forecast loop GIF will be created for each forecast product.

You will need to load Anaconda first. Here are some examples of how to do this:

In an HPC, you can use modules to load Anaconda. Otherwise, use the following instructions. Find download link here: https://www.anaconda.com/products/individual. Scroll to the bottom to the “Anaconda Installers” section and copy the download link for the appropriate version
$ cd $HOME
$ wget <download_link>
$ bash <Anaconda_excecutable_name>
$ export PATH=$HOME/anaconda3/bin:$PATH

Install Libraries
$ conda install -c conda-forge -y cartopy
$ conda install -y netCDF4

Download Files
$ cd $SCRATCH

Get natural earth files
$ wget https://ftp.emc.ncep.noaa.gov/EIB/UFS/SRW/v1p0/natural_earth/natural_earth_ufs-srw-release-v1.0.0.tar.gz
$ tar -xzf natural_earth_ufs-srw-release-v1.0.0.tar.gz
$ wget <TODO_new_URL>

Run Script
Navigate to the run directory of your experiment
$ cd $SCRATCH/<expt_name>/run

Create symbolic link to plotting script
$ ln -sf dir/of/plot_mrw.py .

Run script
$ python3 plot_mrw.py (Start time)<YYYYMMDDHH> (Start Forecast hour)<HHH> (End Forecast hour)<HHH> <Natural Earth Directory>
For example
$ python3 plot_mrw.py 2019082900 000 048 $SCRATCH/natural_earth


View Results
A utility typically used to visualize the resulting images in png format is display. If it is available on your platform, you can use the command:
$ display *.png

If you are working on a remote platform and want to view the GIFs and files, you could also use scp to transfer files to your local computer.


Sample Output
The sample plots are in the sample_output.pdf document in this folder. They are consistent with the Hurricane Dorian initial conditions and tag ufs-v1.1.0. Your results may look different if you are using a different branch or tag. Your results will also look different just because you are running on a platform different from what we used to generate the plots.
Now that you completed this step, you may be interested in trying to change a namelist option and run a second test to check your dexterity and understanding of how results will change. If you are interested in doing that, please visit the UFS Portal to take our graduate student test, which will give you instructions to take that leap, and will also provide important information for our development work







Plot Results from MRW Graduate Student Test (GST)
====================================================

The ``plot_mrw_cloud_diff.py`` generates plots of cloud cover from the UFS MRW App Graduate Student Test (GST) control, experiment, and difference between control and experiment.

################################################################################

# Instructions:		Make sure all the necessary modules can be imported.
#                       Five command line arguments are needed:
#                       1. Cycle date/time in YYYYMMDDHH format
#                       2. Forecast hour in HHH format
#                       3. CTRL_DIR: Control directory
#                          -Postprocessed data should be found in the directory:
#                            CTRL_DIR/run/
#                       4. EXPT_DIR: Experiment directory
#                          -Postprocessed data should be found in the directory:
#                            EXPT_DIR/run/
#                       5. CARTOPY_DIR:  Base directory of cartopy shapefiles
#                          -Shapefiles cannot be directly downloaded to NOAA
#                            machines from the internet, so shapefiles need to
#                            be downloaded if geopolitical boundaries are
#                            desired on the maps.
#                          -File structure should be:
#                            CARTOPY_DIR/shapefiles/natural_earth/cultural/*.shp
#                          -More information regarding files needed to setup
#                            display maps in Cartopy, see SRW App Users' Guide
#                            https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.0/
#
#                       Plots are saved in the same directory that this script 
#                           is in as YYYYMMDDHH<variable>_<domain>_fHHH.png
#
#                       The variable domains in this script can be set to either
#                           'conus' for a CONUS map or manually adjusted in the 
#                           # Domain section
#
################################################################################



################################################################################
####  Python Script Documentation Block
#
# Script name:       	plot_mrw.py
# Script description:  	Generates plots of several output variables from UFS 
#           MRW App Graduate Student Test
#
# Authors:  Sam Ephraim		Org: EPIC Lapenta Intern		Date: 2021-07-06
#           Based off of work from Ben Blake and David Wright
#
# Instructions:		Make sure all the necessary modules can be imported.
#                       Five command line arguments are needed:
#                       1. Cycle date/time in YYYYMMDDHH format
#                       2. Start forecast hour in HHH format
#                       3. End forecast hour in HHH format
#                       4. CARTOPY_DIR:  Base directory of cartopy shapefiles
#                          -Shapefiles cannot be directly downloaded to NOAA
#                            machines from the internet, so shapefiles need to
#                            be downloaded if geopolitical boundaries are
#                            desired on the maps.
#                          -File structure should be:
#                            CARTOPY_DIR/shapefiles/natural_earth/cultural/*.shp
#                          -More information regarding files needed to setup
#                            display maps in Cartopy, see SRW App Users' Guide
#                            https://ufs-srweather-app.readthedocs.io/en/ufs-v1.0.0/
#
#                       Plots are saved in the same directory that this script 
#                           is in as YYYYMMDDHH<variable>_<domain>_fHHH.png
#
#                       The variable domains in this script can be set to either
#                           'conus' for a CONUS map or manually adjusted in the 
#                           # Domain section
#
#
################################################################################




