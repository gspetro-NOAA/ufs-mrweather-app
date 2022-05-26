.. _quickstart:

====================
Workflow Quick Start
====================

This Quick Start Guide will help users to build and run the "out-of-the-box" case for the Unified Forecast System (:term:`UFS`) `Medium-Range Weather (MRW) Application <https://github.com/ufs-community/ufs-mrweather-app>`__ on preconfigured (`Level 1 <https://github.com/ufs-community/ufs/wiki/Supported-Platforms-and-Compilers>`__) machines. This chapter uses :term:`uncoupled`, :term:`free-forecast` mode. For more detailed guidance on building and running the MRW App, see Chapters :numref:`%s <build-mrw>` and :numref:`%s <run-mrw>`. 

..
   COMMENT: Describe case we are running! Hurricane?

Building the UFS Medium-Range Weather Application 
=========================================================

#. Clone the MRW App from GitHub:

   .. code-block:: console

      git clone -b master https://github.com/ufs-community/ufs-mrweather-app.git

   ..
      COMMENT: Change branch for release.

#. Check out the external repositories:

   .. code-block:: console

      cd ufs-mrweather-app
      ./manage_externals/checkout_externals

#. Build the global-workflow components (including the UFS Weather Model):

   .. code-block:: console
      
      sh build_global-workflow.sh

Running the UFS Medium-Range Weather Application 
=========================================================

#. Download and stage data according to the instructions in :numref:`Chapter %s <downloading_input_data>` (if using new data or when operating on a Level 2-4 system).

#. From the ``global-workflow/ush/rocoto`` directory, load the appropriate modules:

   .. code-block:: console
   
      cd ufs-mrweather-app/global-workflow/ush/rocoto

   On Orion:

   .. code-block:: console
      
      module load contrib/0.1
      module load rocoto/1.3.3

   ..
      COMMENT: Should it be module USE contrib/0.1???

   On Hera:

   .. code-block:: console
      
      module use -a /contrib/anaconda/modulefiles
      module load anaconda/anaconda3-5.3.1

   On other Level 1 systems, users can run ``module spider`` to view the location of the rocoto modules. 

   ..
      COMMENT: Do they only need rocoto or other modules, too?

#. Run the ``./setup_expt.py`` experiment generator script:

   .. code-block:: console
   
      ./setup_expt.py forecast-only --pslot <experiment_name> --idate <YYYYMMDDHH> --edate <YYYYMMDDHH> --resdet <desired_resolution> --gfs_cyc <\#> --comrot <PATH_TO_YOUR_COMROT_DIR> --expdir <PATH_TO_YOUR_EXPDIR>

   For example: 

   .. code-block:: console
      
      ./setup_expt.py forecast-only --pslot test --idate 2020010100 --edate 2020010118 --resdet 384 --gfs_cyc 4 --comrot /work/noaa/stmp/cbook/COMROT --expdir /work/noaa/epic-ps/cbook/uncoupled/EXPDIR

   ..
      COMMENT: Should probably use different sample paths or otherwise replace cbook with ilastname. 

   .. attention::

      ``--idate`` and ``--edate`` must be the *same* when running in :term:`free-forecast` mode and must refer to the initial start time of the experiment. 

   This will generate $PSLOT (specific experiment name) folders in ``COMROT`` and ``EXPDIR``, with a collection of ``config`` files in ``$EXPDIR/$PSLOT``.

#. Copy initial conditions (IC) files into ``$COMROT/$PSLOT``. 

   .. code-block:: console
      
      cp <ICfile> $COMROT/$PSLOT
   
   where <ICfile> refers to a given IC file. An entire directory of IC files can be copied by adding the ``-r`` argument. These files should be within a directory named following the ``gfs.YYYYMMDDHH`` convention with a filename structure like ``gfs.$YYYYMMDD/HH/atmos``. The INPUT folder within ``.../atmos/`` contains ``sfc`` files needed for the GFS ATM to run.

   ..
      COMMENT: What is the GFS ATM? GFS atmospheric model? What is the INPUT folder?

#. Edit ``config.base`` in ``$EXPDIR/$PSLOT``. In particular, users will need to modify the following parameters: ACCOUNT, HOMEDIR, STMP, PTMP, HPSSARCH, SDATE, EDATE, and the length of the forecast. 

   ..
      COMMENT: HOW to update length of forecast??? What variable?

#. Run the following to generate a crontab and ``.xml`` files for the experiment in ``$EXPDIR/$PSLOT``:

   .. code-block:: console
      
      ./setup_workflow_fcstonly.py --expdir $EXPDIR/$PSLOT

#. Submit job through crontab by copying entry in ``$PSLOT.crontab`` into crontab via ``crontab -e``.

#. Monitor status of workflow using rocotostat:

   .. code-block:: console
      
      rocotostat -d </path/to/workflow/database/file> -w </path/to/workflow/xml/file> [-c YYYYMMDDHHmm,[YYYYMMDDHHmm,...]] [-t taskname,[taskname,...]] [-s] [-T]
   
   where ``-c`` and ``-t`` are optional arguments referring to the cycle and task name, respectively. 

   ..
      COMMENT: What are the -s and -T options?

   For example: 

   .. code-block:: console
      
      rocotostat -d $PSLOT.db -w $PSLOT.xml

#. Check status of specific task/job:

   .. code-block:: console
      
      rocotocheck -d </path/to/workflow/database/file> -w </path/to/workflow/xml/file> -c <YYYYMMDDHHmm> -t <taskname>
   
   ..
      COMMENT: Provide concrete example?


