.. _build-mrw:

===========================================================
Build the Medium-Range Weather (MRW) Application 
===========================================================

The Unified Forecast System (:term:`UFS`) Medium-Range Weather (MRW) Application is an :term:`umbrella repository` consisting of a number of different :ref:`components <Components>` housed in external repositories. Once the MRW App is configured and built, users can generate predictions of atmospheric behavior out to about two weeks. 

This chapter walks users through how to build and run the "out-of-the-box" case for the MRW App. However, the steps are relevant to any MRW Application experiment and can be modified to suit users' goals. The "out-of-the-box" MRW App case builds a weather forecast for _____DATES_____. The forecast uses the ___________ domain, the Global Forecast System (:term:`GFS`) version 16 physics suite (FV3_GFS_v16 :term:`CCPP`), and :term:`FV3`-based GFS raw external model data for initialization.

..
   COMMENT: Replace "Multiple convective weather events during these two days produced over 200 filtered storm reports. Severe weather was clustered in two areas: the Upper Midwest through the Ohio Valley and the Southern Great Plains. This forecast uses a predefined 25-km Continental United States (:term:`CONUS`) domain (RRFS_CONUS_25km)" with MRW-specific details

.. attention::

   All UFS applications support `four platform levels <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`__. The steps described in this chapter will work most smoothly on preconfigured (Level 1) systems. On Level 1 systems, all of the required libraries for building community releases of UFS models and applications are available in a central location. This guide can serve as a starting point for running the MRW App on other systems, too, but users may need to perform additional troubleshooting. 

..
   COMMENT: Are we using the container approach for the MRW?
   .. note::
      The :ref:`container approach <QuickstartC>` is recommended for a smoother build and run experience. Building without a container allows for the use of the Rocoto workflow manager and may allow for more customization. However, the non-container approach requires more in-depth system-based knowledge, especially on Level 3 and 4 systems; it is less appropriate for beginners. 

The steps required for building the MRW App are as follows:

   * :ref:`Install prerequisites <spack-stack-info>`
   * :ref:`Clone the SRW App from GitHub <DownloadMRWApp>`
   * :ref:`Check out the external repositories <checkout-externals>`
   * :ref:`Set up the build environment and build the executables <setenv-build>`
   
..   
   * :ref:`Generate a regional workflow experiment <GenerateForecast>`
      * :ref:`Configure the experiment parameters <UserSpecificConfig>`
      * :ref:`Load the python environment for the regional workflow <SetUpPythonEnv>`

..
   COMMENT: If time, create workflow image as in SRW _static/FV3LAM_wflow_overall.png file. 
..
   COMMENT: Edit section to make it MRW-specific!!! Thus far just copy-pasted from SRW. 

.. _spack-stack-info:

Install *spack-stack*
=======================

.. Attention::
   Skip the *spack-stack* installation if working on a `Level 1 system <https://github.com/ufs-community/ufs-srweather-app/wiki/Supported-Platforms-and-Compilers>`__ (e.g., Cheyenne, Hera, Orion, NOAA Cloud); *spack-stack* is already installed there.

**Definition:** :term:`spack-stack` is a repository that provides a Spack-based method for building the software stack required to run numerical weather prediction (NWP) tools such as the MRW App and other UFS applications. *spack-stack* uses the Spack package manager along with custom Spack configuration files and Python scripts to simplify installation of the libraries required to run various applications. 

Background
----------------

The UFS Weather Model draws on over 50 code libraries to run its applications, including the MRW Application. These libraries range from libraries developed in-house at NOAA (e.g., NCEPLIBS, FMS) to libraries developed by NOAA's partners (e.g., PIO, ESMF) to truly third party libraries (e.g., NETCDF). Individual installation of these libraries is not practical, so the `spack-stack <https://github.com/NOAA-EMC/spack-stack>`__ was developed as a central installation system to ensure that the infrastructure environment across multiple platforms is as similar as possible. Installation of the *spack-stack* is required to run the MRW App. 

Instructions
-------------------------
Users working on systems that fall under `Support Levels 2-4 <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`__ will need to install *spack-stack* the first time they try to build applications (such as the MRW App) that depend on it. Users can either build the *spack-stack* on their local system or use the centrally maintained stacks on each HPC platform if they are working on a Level 1 system. For a detailed description of installation options, see *:ref:`Installing spack-stack <InstallBuildSpackStack>`*. 

After completing installation, continue to the next section.

.. _DownloadMRWApp:

Download the UFS MRW Application Code
========================================

The MRW Application source code is publicly available on GitHub. To download the MRW App, clone the ``master`` branch of the repository:

.. code-block:: console

   git clone -b master https://github.com/ufs-community/ufs-mrweather-app.git

..
   COMMENT: This will need to be changed to the updated release branch of the MRW repo once it exists. 

The cloned repository contains the configuration files and sub-directories shown in
:numref:`Table %s <FilesAndSubDirs>` below.

.. _FilesAndSubDirs:

.. table:: Files and sub-directories of the ufs-mrweather-app repository

   +--------------------------+--------------------------------------------------------------+
   | **File/Directory Name**  | **Description**                                              |
   +==========================+==============================================================+
   | Externals.cfg            | Includes tags pointing to the correct version of the         |
   |                          | external GitHub repositories/branches used in the MRW App.   |
   +--------------------------+--------------------------------------------------------------+
   | LICENSE.md               | CC0 license information                                      |
   +--------------------------+--------------------------------------------------------------+
   | README.md                | Getting Started Guide                                        |
   +--------------------------+--------------------------------------------------------------+
   | build_global-workflow.sh | Script to build the global workflow for the MRW App.         |
   +--------------------------+--------------------------------------------------------------+
   | describe_version         | Script for describing the CESM version and any local         |
   |                          | modifications                                                |
   +--------------------------+--------------------------------------------------------------+
   | docs                     | Contains release notes, documentation, and User's Guide      |
   +--------------------------+--------------------------------------------------------------+
   | manage_externals         | Utility for checking out external repositories               |
   +--------------------------+--------------------------------------------------------------+
   | plotting_scripts         | Scripts and instructions for plotting the results of a       |
   |                          | forecast in order to visually check the results for 2-m      |
   |                          | temperature, hourly precipitation, cloud cover, and 10-m     |
   |                          | wind at a user inputted time range.                          |
   +--------------------------+--------------------------------------------------------------+

..
   COMMENT: Table above is all set for MRW! :)

.. _checkout-externals:

Check Out External Components
================================

The MRW App relies on the global workflow and its components, detailed in :numref:`Section %s <components>` of this User's Guide. Each component has its own :term:`repository`. Users must run the ``checkout_externals`` script to collect the individual components of the MRW App from their respective git repositories. The ``checkout_externals`` script uses the configuration file ``Externals.cfg`` in the top level directory of the MRW App to clone the correct tags (code versions) of the external repositories listed in :numref:`Section %s <hierarchical-repo-str>` into the appropriate directories under the ``global-workflow`` and ``src`` directories. 

Run the executable that pulls in MRW App components from external repositories:

Checkout the global workflow:

.. code-block:: console
   
   cd ufs-mrweather-app
   ./manage_externals/checkout_externals

Determine whether checkout of externals was successful using:

.. code-block:: console
   
   ./manage_externals/checkout_externals -S

.. _setenv-build:

Set Up the Environment and Build the Executables
===================================================

The MRW App requires certain directories to hold experiment input and output. For 


Set-up (uncoupled, free-forecast mode w/ cold-start initial conditions)

Build UFS model and global-workflow components

.. code-block:: console

   sh build_global-workflow.sh [-c]

..
   COMMENT: List options (in shell script, email from Walter or global-workflow page)!!!
   -a S2S
   -a atm

.. note::
   Only use the -c option to compile for coupled UFS; requires different physics packages and APP argument when running setup_expt.py in step 5.

