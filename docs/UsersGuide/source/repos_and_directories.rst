.. _repos_and_directories:

=========================================
Code repositories and directory structure
=========================================

This chapter describes the code repositories that comprise the MRW App,
without describing, in detail, any of the components.

Hierarchical Repository Structure
---------------------------------

The :term:`umbrella repository` for the MRW Application is named ``ufs-mrweather-app`` and is available on GitHub at https://github.com/ufs-community/ufs-mrweather-app. An umbrella repository is a repository that includes links, called *externals*, to additional repositories. The MRW App includes the ``checkout_externals`` tools along with a configuration file called ``Externals.cfg``, which describes the external repositories associated with the MRW App umbrella repository (see :numref:`Table %s <top_level_repos>`).

.. _top_level_repos:

.. table::  List of top-level repositories included in the MRW App

   +-------------------------------+---------------------------------------------------------+
   | **Repository Description**    | **Authoritative repository URL**                        |
   +===============================+=========================================================+
   | Umbrella repository for       | https://github.com/ufs-community/ufs-mrweather-app      |
   | the MRW App                   |                                                         |
   +-------------------------------+---------------------------------------------------------+
   | Umbrella repository for       | https://github.com/ufs-community/ufs-weather-model      |
   | the UFS Weather Model         |                                                         |
   +-------------------------------+---------------------------------------------------------+
   | Repository for the global     | https://github.com/NOAA-EMC/global-workflow             |
   | workflow                      |                                                         |
   +-------------------------------+---------------------------------------------------------+
   | Repository for UFS utilities, | https://github.com/ufs-community/UFS_UTILS              |
   | (e.g., pre-processing,        |                                                         |
   | chgres_cube)                  |                                                         |
   +-------------------------------+---------------------------------------------------------+
   | Repository for the Unified    | https://github.com/NOAA-EMC/UPP                         |
   | Post Processor (UPP)          |                                                         |
   +-------------------------------+---------------------------------------------------------+

..
   COMMENT: Add GW link. 

The UFS Weather Model is itself an umbrella repository and contains a number of sub-repositories
used by the model as documented `here <https://ufs-weather-model.readthedocs.io/en/ufs-v1.1.0/CodeOverview.html>`__. 

Note that the prerequisite libraries (including NCEP Libraries and external libraries) are not included in the UFS MRW Application repository. The *`spack-stack <https://github.com/NOAA-EMC/spack-stack>`__* repository assembles these prerequisite libraries. The *spack-stack* has already been built on `preconfigured (Level 1) platforms <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`__. However, it must be built on other systems. :numref:`Chapter %s <install-build-spack-stack>` contains details on installing the *spack-stack*. 

.. _TopLevelDirStructure:

Directory Structure
-------------------

The directory structure on disk for users of the MRW App depends on whether one is using
a pre-configured platform. Users working on pre-configured platforms will only have the
files associated with the ufs-mrweather-app in their disk space. The directory structure is set
in configuration file ``Externals.cfg``, which is in the top directory where the umbrella repository
has been cloned. A listing of the directory structure is shown :ref:`here <top_level_dir_structure>`.

The directory structures for the standalone UFS Weather Model and the UFS Weather Model included with
the MRW App are equal in that they contain subdirectories for :term:`FMS`, :term:`FV3`, :term:`NEMS`
and stochastic_physics. However, in the MRW App, subdirectories are located under ``src/model``.
The MRW App also includes directories for CIME, such as the ``src/model/FV3/cime`` and
``src/model/NEMS/cime`` directories.

Users working outside of preconfigured platforms will have additional files on disk associated with
the libraries, pre- and post-processing.  The resulting directory structure is determined by the path
settings in the NCEPLIBS ``.gitmodules`` file.

..
   COMMENT: Edit section above for accuracy...


The ``ufs-mrweather-app`` :term:`umbrella repository` structure is determined by the ``local_path`` settings contained within the ``Externals.cfg`` file. After ``manage_externals/checkout_externals`` is run (:numref:`Step %s <checkout-externals>`), the specific GitHub repositories described in :numref:`Table %s <top_level_repos>` are cloned into the target subdirectories shown below. Directories that will be created as part of the build process appear in parentheses and will not be visible until after the build is complete. Some directories have been removed for brevity.

.. code-block:: console

   ufs-mrweather-app
   ├── (bin)
   ├── (build)
   ├── docs  
   │     └── UsersGuide
   ├── (include)
   ├── (lib)
   ├── manage_externals
   ├── global-workflow
   │     ├── docs
   │     │     └── UsersGuide
   │     ├── (fix)
   │     ├── jobs
   │     ├── modulefiles
   │     ├── scripts
   │     ├── tests
   │     │     └── baseline_configs
   │     └── ush
   │          ├── Python
   │          ├── rocoto
   │          ├── templates
   │          └── wrappers
   ├── (share)
   ├── plotting_scripts
   └── src
        ├── UPP
        │     ├── parm
        │     └── sorc
        │          └── ncep_post.fd
        ├── UFS_UTILS
        │     ├── sorc
        │     │    ├── chgres_cube.fd
        │     │    ├── fre-nctools.fd
        |     │    ├── grid_tools.fd
        │     │    ├── orog_mask_tools.fd
        │     │    └── sfc_climo_gen.fd
        │     └── ush
        └── ufs_weather_model
    	     └── FV3
                  ├── atmos_cubed_sphere
                  └── ccpp

..
   COMMENT: Update directory tree above to reflect MRW, not SRW!!!

Global Workflow Sub-Directories
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
A number of sub-directories are created under the ``global-workflow`` directory when the global workflow is cloned (see directory diagram :ref:`above <TopLevelDirStructure>`). :numref:`Table %s <Subdirectories>` describes the contents of these sub-directories. 

.. _Subdirectories:

.. table::  Sub-directories of the global workflow

   +-------------------------+---------------------------------------------------------+
   | **Directory Name**      | **Description**                                         |
   +=========================+=========================================================+
   | docs                    | User's Guide Documentation                              |
   +-------------------------+---------------------------------------------------------+
   | jobs                    | J-job scripts launched by Rocoto                        |
   +-------------------------+---------------------------------------------------------+
   | modulefiles             | Files used to load modules needed for building and      |
   |                         | running the workflow                                    |
   +-------------------------+---------------------------------------------------------+
   | scripts                 | Run scripts launched by the J-jobs                      |
   +-------------------------+---------------------------------------------------------+
   | tests                   | Baseline experiment configuration                       |
   +-------------------------+---------------------------------------------------------+
   | ush                     | Utility scripts used by the workflow                    |
   +-------------------------+---------------------------------------------------------+

..
   COMMENT: Edit table to reflect actual MRW config

.. _ExperimentDirSection:

Experiment Directory Structure
--------------------------------
When the user generates an experiment (:numref:`Step %s <expt-gen>`), a user-defined experimental directory (``EXPTDIR``) is created based on information specified in the ``config.sh`` file. :numref:`Table %s <ExptDirStructure>` shows the contents of the experiment directory before running the experiment workflow.

..
   COMMENT: Is this accurate?

.. _ExptDirStructure:

.. table::  Files and sub-directory initially created in the experimental directory 
   :widths: 33 67 

   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | **File Name**             | **Description**                                                                                       |
   +===========================+=======================================================================================================+
   | config.sh                 | User-specified configuration file, see :numref:`Section %s <UserSpecificConfig>`                      |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | data_table                | Cycle-independent input file (empty)                                                                  |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | field_table               | Tracers in the `forecast model                                                                        |
   |                           | <https://ufs-weather-model.readthedocs.io/en/ufs-v2.0.0/InputsOutputs.html#field-table-file>`_        |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | FV3LAM_wflow.xml          | Rocoto XML file to run the workflow                                                                   |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | input.nml                 | Namelist for the `UFS Weather model                                                                   |
   |                           | <https://ufs-weather-model.readthedocs.io/en/ufs-v2.0.0/InputsOutputs.html#namelist-file-input-nml>`_ | 
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | launch_FV3LAM_wflow.sh    | Symlink to the shell script of                                                                        |
   |                           | ``ufs-srweather-app/regional_workflow/ush/launch_FV3LAM_wflow.sh``                                    |
   |                           | that can be used to (re)launch the Rocoto workflow.                                                   |
   |                           | Each time this script is called, it appends to a log                                                  |
   |                           | file named ``log.launch_FV3LAM_wflow``.                                                               |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | log.generate_FV3LAM_wflow | Log of the output from the experiment generation script                                               |
   |                           | ``generate_FV3LAM_wflow.sh``                                                                          |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | nems.configure            | See `NEMS configuration file                                                                          |
   |                           | <https://ufs-weather-model.readthedocs.io/en/ufs-v2.0.0/InputsOutputs.html#nems-configure-file>`_     |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | suite_{CCPP}.xml          | CCPP suite definition file used by the forecast model                                                 |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   | var_defns.sh              | Shell script defining the experiment parameters. It contains all                                      |
   |                           | of the primary parameters specified in the default and                                                |
   |                           | user-specified configuration files plus many secondary parameters                                     |
   |                           | that are derived from the primary ones by the experiment                                              |
   |                           | generation script. This file is sourced by various other scripts                                      |
   |                           | in order to make all the experiment variables available to these                                      |
   |                           | scripts.                                                                                              |
   +---------------------------+-------------------------------------------------------------------------------------------------------+
   |  YYYYMMDDHH               | Cycle directory (empty)                                                                               |
   +---------------------------+-------------------------------------------------------------------------------------------------------+

..
   COMMENT: Edit table to reflect MRW
   COMMENT: SRW page says: "In addition, running the SRW App in *community* mode creates the ``fix_am`` and ``fix_lam`` directories in ``EXPTDIR``. The ``fix_lam`` directory is initially empty but will contain some *fix* (time-independent) files after the grid, orography, and/or surface climatology generation tasks are run." Is there something similar for MRW App?

.. _FixDirectories:

.. table::  Description of the fix directories

   +-------------------------+----------------------------------------------------------+
   | **Directory Name**      | **Description**                                          |
   +=========================+==========================================================+
   | fix_am                  | Directory containing the global fix (time-independent)   |
   |                         | data files. The experiment generation script copies      |
   |                         | these files from a machine-dependent system directory.   |
   +-------------------------+----------------------------------------------------------+
   | fix_lam                 | Directory containing the regional fix (time-independent) |
   |                         | data files that describe the regional grid, orography,   |
   |                         | and various surface climatology fields as well as        |
   |                         | symlinks to pre-generated files.                         |
   +-------------------------+----------------------------------------------------------+

..
   COMMENT: Edit table to reflect MRW
   COMMENT: SRW page says: "Once the workflow is launched with the ``launch_FV3LAM_wflow.sh`` script, a log file named ``log.launch_FV3LAM_wflow`` will be created (unless it already exists) in ``EXPTDIR``. The first several workflow tasks (i.e., ``make_grid``, ``make_orog``, ``make_sfc_climo``, ``get_extrn_ics``, and ``get_extrn_lbc``) are preprocessing tasks, which result in the creation of new files and sub-directories, described in :numref:`Table %s <CreatedByWorkflow>`." Is there something similar for MRW?

.. _CreatedByWorkflow:

.. table::  New directories and files created when the workflow is launched
   :widths: 30 70

   +---------------------------+--------------------------------------------------------------------+
   | **Directory/File Name**   | **Description**                                                    |
   +===========================+====================================================================+
   | YYYYMMDDHH                | This is a “cycle directory” that is updated when the first         |
   |                           | cycle-specific workflow tasks (``get_extrn_ics`` and               |
   |                           | ``get_extrn_lbcs``) are run. These tasks are launched              |
   |                           | simultaneously for each cycle in the experiment. Cycle directories |
   |                           | are created to contain cycle-specific files for each cycle that    |
   |                           | the experiment runs. If ``DATE_FIRST_CYCL`` and ``DATE_LAST_CYCL`` |
   |                           | are different, and/or if ``CYCL_HRS`` contains more than one       |
   |                           | element in the ``config.sh`` file, more than one cycle directory   |
   |                           | will be created under the experiment directory.                    |
   +---------------------------+--------------------------------------------------------------------+
   | grid                      | Directory generated by the ``make_grid`` task to store grid files  |
   |                           | for the experiment                                                 |
   +---------------------------+--------------------------------------------------------------------+
   | log                       | Contains log files generated by the overall workflow and by its    |
   |                           | various tasks. Look in these files to trace why a task may have    |
   |                           | failed.                                                            |
   +---------------------------+--------------------------------------------------------------------+
   | orog                      | Directory generated by the ``make_orog`` task containing the       |
   |                           | orography files for the experiment                                 |
   +---------------------------+--------------------------------------------------------------------+
   | sfc_climo                 | Directory generated by the ``make_sfc_climo`` task containing the  |
   |                           | surface climatology files for the experiment                       |
   +---------------------------+--------------------------------------------------------------------+
   | FV3LAM_wflow.db           | Database files that are generated when Rocoto is called (by the    |
   | FV3LAM_wflow_lock.db      | launch script) to launch the workflow.                             |
   +---------------------------+--------------------------------------------------------------------+
   | log.launch_FV3LAM_wflow   | The ``launch_FV3LAM_wflow.sh`` script appends its output to this   |
   |                           | log file each time it is called. Take a look at the last 30–50     |
   |                           | lines of this file to check the status of the workflow.            |
   +---------------------------+--------------------------------------------------------------------+

The output files for an experiment are described in :numref:`Section %s <output>`.
The workflow tasks are described in :numref:`Section %s <WorkflowTaskDescription>`.

..
   COMMENT: Edit table/section above to reflect MRW!
