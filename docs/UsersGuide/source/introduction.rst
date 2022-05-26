.. _introduction:

=============
Introduction
=============

The Unified Forecast System (:term:`UFS`) is a community-based, coupled, comprehensive Earth modeling system. NOAA’s operational model suite for numerical weather prediction (NWP) is quickly transitioning to the UFS from a number of different modeling systems. The UFS enables research, development, and contribution opportunities within the broader :term:`weather enterprise` (e.g. government, industry, and academia). For more information about the UFS, visit the `UFS Portal <https://ufscommunity.org/>`__.

The UFS includes `multiple applications <https://ufscommunity.org/science/aboutapps/>`__ that span local to global domains and a range of predictive time scales. This documentation describes the UFS Medium-Range Weather (MRW) Application (App), which targets predictions of atmospheric behavior out to about two weeks. This MRW App release includes a prognostic atmospheric model, pre- and post-processing tools, and a community workflow. These components are documented within this User's Guide and supported through a `community forum <https://forums.ufscommunity.org/>`__. Additionally, the MRW App has transitioned from a :term:`CIME`-based workflow to the `global workflow <https://github.com/NOAA-EMC/global-workflow/>`__. New and improved capabilities for the upcoming release include the addition of a verification package (METplus) for both deterministic and ensemble simulations and support for four Stochastically Perturbed Perturbation (SPP) schemes. Future work will expand the capabilities of the application to include data assimilation (DA) and a forecast restart/cycling capability.

The MRW App is `available on GitHub <https://github.com/ufs-community/ufs-mrweather-app.git>`__ and is designed to be code that the community can run and improve. It is portable to a set of `commonly used platforms <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`__. A limited set of configurations of the release, such as specific model resolutions and physics options, are documented and supported. This documentation provides a :ref:`Quick Start Guide <quickstart>` for running the MRW Application and a detailed guide for running the MRW App on supported platforms. It also provides an overview of the release components and details on how to customize or modify different portions of the workflow.

..
   COMMENT: Add: The MRW App v2.0.0 citation is as follows and should be used when presenting results based on research conducted with the App: UFS Development Team. (2022, June __). Unified Forecast System (UFS) Medium-Range Weather (MRW) Application (Version v2.0.0). Zenodo. https://doi.org/.........

..
   COMMENT: Update release number/links; remove reference to "upcoming" release.
   COMMENT: Is the "future work" section accurate?
   COMMENT: Add v2.0.0 wiki page!

How to Use This Document
========================

This guide instructs both novice and experienced users on downloading, building, and running the MRW Application. Please post questions in the `UFS Forum <https://forums.ufscommunity.org/>`__.

.. code-block:: console

   Throughout the guide, this presentation style indicates shell commands and options, 
   code examples, etc.

Variables presented as ``AaBbCc123`` in this User's Guide typically refer to variables in scripts, names of files, and directories.

File paths or code that include angle brackets (e.g., ``build_<platform>_<compiler>.env``) indicate that users should insert options appropriate to their MRW App configuration (e.g., ``build_orion_intel.env``). 

..
   COMMENT: Change examples to be MRW-specific.

.. hint:: 
   * To get started running the MRW App, see the :ref:`Quick Start Guide <quickstart>` for beginners or refer to the in-depth chapter on :ref:`Running the Medium-Range Weather Application <build-mrw>`. 
   * For background information on the MRW App code repositories and directory structure, see :numref:`Section %s <MRWStructure>` below. 
   * For an outline of MRW App components, see section :numref:`Section %s <components-overview>` below or refer to :numref:`Chapter %s <components>` for a more in-depth treatment.

.. note::

   Variables presented as ``$VAR`` in this guide typically refer to variables in XML files
   in a MRW App experiment. 

..
   COMMENT: Remove note?

.. _MRWPrerequisites:

Prerequisites for Using the MRW Application
===============================================

Background Knowledge Prerequisites
--------------------------------------

The instructions in this documentation assume that users have certain background knowledge: 

* Familiarity with LINUX/UNIX systems
* Command line basics
* System configuration knowledge (e.g., compilers, environment variables, paths, etc.)
* Numerical Weather Prediction
* Meteorology

..
   COMMENT: Add subpoints!

Additional background knowledge in the following areas could be helpful:
* High-Performance Computing (HPC) Systems for those running the MRW App on an HPC system
* Programming (particularly Python) for those interested in contributing to the MRW App code
* Creating an SSH Tunnel to access HPC systems from the command line
* Containerization
* Workflow Managers/Rocoto

..
   COMMENT: Eliminate containerization?


Software/Operating System Requirements
-----------------------------------------
The UFS MRW Application has been designed so that any sufficiently up-to-date machine with a UNIX-based operating system should be capable of running the application. NOAA `Level 1 & 2 systems <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`__ already have these prerequisites installed. However, users working on other systems must ensure that the following requirements are installed on their system: 

**Minimum Platform Requirements:**

   * UNIX style operating system such as CNL, AIX, Linux, Mac

   ..
      COMMENT: Does it need to be POSIX-compliant, too, as /wSRW, or is that implied? 

   * * >40 GB disk space

      * 18 GB input data from GFS, RAP, and HRRR for "out-of-the-box" MRW App case described in :numref:`Chapter %s <build-mrw>`
      * 6 GB for :term:`spack-stack` full installation
      * 1 GB for ufs-mrweather-app installation
      * 11 GB for 120hr/5-day forecast 
   
   * 4GB memory (25km domain)

   ..
      COMMENT: CHANGE/REVISE all numbers above to correspond to MRW!!!
      COMMENT: How large is basic input data for out-of-the-box case? 
      COMMENT: Where does data come from for out-of-the-box case? Probs no RAP or HRRR...
      COMMENT: How much disk space required for spack-stack? For ufs-mrweather-app installation? For forecast? 

   * Python 3.7+

   * Perl 5

   * Git client (1.8+)

   * Fortran compiler released since 2018

      * gfortran v9+ or ifort v18+ are the only ones tested, but others may work.

   * C compiler compatible with the Fortran compiler

      * gcc v9+, ifort v18+, and clang v9+ (macOS, native Apple clang or LLVM clang) have been tested

   ..
      COMMENT: Should it be C AND C++???
      COMMENT: Have all of these versions been tested...?

The following software is also required to run the MRW Application, but :term:`spack-stack` (which contains the software libraries necessary for building and running the MRW App) can be configured to build these requirements:

   * :term:`MPI` (MPICH, OpenMPI, or other implementation)

      * Only **MPICH** can be built with spack-stack. Other options must be installed separately by the user. 
   
   * `CMake 3.15+ <http://www.cmake.org/>`__

   ..
      COMMENT: Check that this is the case for spack-stack, not just HPC-Stack.

   * `spack-stack <https://github.com/NOAA-EMC/spack-stack>`__ (or `HPC-Stack <https://github.com/NOAA-EMC/hpc-stack>`__), which includes:

      * `NCEPLIBS-external <https://github.com/NOAA-EMC/NCEPLIBS-external>`__ (includes ESMF)
      * `NCEPLIBS <https://github.com/NOAA-EMC/NCEPLIBS>`__

   ..
      COMMENT: Are more software packages required? Should NCEPLIBS, etc. be listed at all???
      COMMENT: Are all of these version numbers up to date?

For MacOS systems, some additional software is needed. It is recommended that users install this software using the `Homebrew <https://brew.sh/>`__ package manager for MacOS:

..
   COMMENT: ADD MacOS-specific software here!!!
   More details are in :numref:`Section %s <genericMacOS>`.

..
   COMMENT: Change above to reflect spack-stack details and/or integrate spack-stack docs.

Optional but recommended prerequisites for all systems:

* Conda for installing/managing Python packages
* Bash v4+
* Rocoto Workflow Management System (1.3.1)
* Python packages ``scipy``, ``matplotlib``, ``pygrib``, ``cartopy``, and ``pillow`` for graphics

..
   COMMENT: Are these packages need for graphics in MRW? or just SRW?

* Lmod

You are now ready to build the MRW App as documented in the :ref:`quickstart`.


.. _components-overview:

MRW App Components Overview 
==============================

Pre-Processor Utilities and Initial Conditions
------------------------------------------------

The MRW App requires input model data and the :term:`chgres_cube` pre-processing software, which is part of the `UFS_UTILS <https://github.com/ufs-community/UFS_UTILS>`__ pre-processing utilities package, to initialize and prepare the model. Additional information about the pre-processor utilities can be found in :numref:`Chapter %s <utils>` and in the `UFS_UTILS User’s Guide <https://noaa-emcufs-utils.readthedocs.io/en/ufs-v2.0.0/>`__.

..
   COMMENT: Check links for paragraph above. 

Forecast Model
--------------------

Atmospheric Model
^^^^^^^^^^^^^^^^^^^^^^
The prognostic atmospheric model in the UFS MRW Application uses the Finite-Volume Cubed-Sphere
(:term:`FV3`) dynamical core. The dynamical core is the computational part of a model that solves the equations of fluid motion for the atmospheric component of the UFS Weather Model. A User’s Guide for the UFS :term:`Weather Model` can be found `here <https://ufs-weather-model.readthedocs.io/en/ufs-v2.0.0/>`__. Additional information about the FV3 dynamical core is available at `here <https://noaa-emc.github.io/FV3_Dycore_ufs-v1.1.0/html/index.html>`__.

Common Community Physics Package
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

The `Common Community Physics Package <https://dtcenter.org/community-code/common-community-physics-package-ccpp>`_ (:term:`CCPP`) supports interoperable atmospheric physics and land surface model options. Atmospheric physics are a set of numerical methods describing small-scale processes such as clouds, turbulence, radiation, and their interactions. The upcoming MRW App release includes four physics suites and :term:`stochastic<Stochastic physics>` options to represent model uncertainty. 

..
   COMMENT: It seems like all but the GFS v16 are designed only for high resolution grids... so why are we including them with this release? It seems like GFS v16 would be more appropriate for the MRW App.

Unified Post-Processor
-------------------------

The Medium-Range Weather (MRW) Application is distributed with a post-processing tool, the `Unified Post Processor <https://dtcenter.org/community-code/unified-post-processor-upp>`__ (:term:`UPP`). The UPP converts the native netCDF output from the model to :term:`GRIB2` format on standard isobaric coordinates in the vertical direction. The UPP can also be used to compute a variety of useful diagnostic fields, as described in the `UPP User’s Guide <https://upp.readthedocs.io/en/upp-v9.0.0/>`__. The UPP output can be used with visualization, plotting and verification packages, or for further downstream post-processing (e.g., statistical post-processing techniques).

.. _Metplus:

METplus Verification Suite
------------------------------

The Model Evaluation Tools (MET) package is a set of statistical verification tools developed by the `Developmental Testbed Center <https://dtcenter.org/>`__ (DTC) for use by the :term:`NWP` community to help them assess and evaluate the performance of numerical weather predictions. MET is the core component of the enhanced METplus verification framework. The suite also includes the associated database and display systems called METviewer and METexpress. METplus spans a wide range of temporal and spatial scales. It is intended to be extensible through additional capabilities developed by the community. More details about METplus can be found in :numref:`Chapter %s <MetplusComponent>` and on the `METplus website <https://dtcenter.org/community-code/metplus>`__.

Visualization Example
--------------------------

This release does not include support for model visualization. Four basic NCAR Command Language (:term:`NCL`) scripts are provided to create a basic visualization of model output, but this capability is provided only as an example for users familiar with NCL. The scripts may be used to complete a visual check to verify that the application is producing reasonable results.

..
   COMMENT: Is this still true?

Workflow and Build System
------------------------------

The MRW Application has a portable CMake-based build system that packages together all the components required to build the MRW Application. Once built, users can generate a Rocoto-based workflow that will run each task in the proper sequence (see `Rocoto documentation <https://github.com/christopherwharrop/rocoto/wiki/Documentation>`__ for more on workflow management). 

..
   COMMENT: Is Linux/Mac still supported? Seems like we're not testing it...

This MRW Application release has been tested on a variety of platforms widely used by researchers, including NOAA High-Performance Computing (HPC) systems (e.g. Jet, Gaea), cloud environments, and generic Linux and macOS systems. Four `levels of support <https://github.com/ufs-community/ufs-mrweather-app/wiki/Supported-Platforms-and-Compilers-for-MRW-App>`_ have been defined for the MRW Application. Preconfigured (Level 1) systems already have the required software libraries (*spack-stack*) available in a central location. The MRW Application is expected to build and run out-of-the-box on these systems, and users can :ref:`download the MRW App code <DownloadMRWApp>` without first installing prerequisites. On other platforms, the required libraries will need to be installed as part of the :ref:`MRW Application build <build-mrw>` process. On Level 2 platforms, installation should be straightforward, and the MRW App should build and run successfully. On Level 3 & 4 platforms, users may need to perform additional troubleshooting since little or no pre-release testing has been conducted on these systems.

..
   COMMENT: What about Level 2 systems?!

.. _MRWStructure:

Code Repositories and Directory Structure
=========================================

.. _hierarchical-repo-str:

Hierarchical Repository Structure
-----------------------------------

..
   COMMENT: Update this from code repos dirs doc!


User Support, Documentation, and Contributing Development
===========================================================
A `forum-based online support system <https://forums.ufscommunity.org>`__ with topical sections
provides a centralized location for UFS users and developers to post questions and exchange information. The forum complements the distributed documentation, summarized here for ease of use.

.. _list_of_documentation:

.. table:: Centralized list of documentation

   +----------------------------+---------------------------------------------------------------------------------+
   | **Documentation**          | **Location**                                                                    |
   +============================+=================================================================================+
   | MRW App v2.0               | https://ufs-mrweather-app.readthedocs.io/en/ufs-v1.1.0                          |
   | User's Guide               |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | chgres_cube User's Guide   | https://ufs-utils.readthedocs.io/en/ufs-v1.1.0                                  |
   +----------------------------+---------------------------------------------------------------------------------+
   | UFS Weather Model v2.0     | https://ufs-weather-model.readthedocs.io/en/ufs-v1.1.0                          |
   | User's Guide               |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | FV3 Documentation          | https://noaa-emc.github.io/FV3_Dycore_ufs-v1.1.0/html/index.html                |
   +----------------------------+---------------------------------------------------------------------------------+
   | CCPP Scientific            | https://dtcenter.org/GMTB/v4.1.0/sci_doc                                        |
   | Documentation              |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | CCPP Technical             | https://ccpp-techdoc.readthedocs.io/en/v4.1.0                                   |
   | Documentation              |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | Stochastic Physics         | https://stochastic-physics.readthedocs.io/en/ufs-v1.1.0                         |
   | User's Guide               |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | ESMF manual                | http://www.earthsystemmodeling.org/esmf_releases/public/ESMF_8_0_0/ESMF_refdoc  |
   +----------------------------+---------------------------------------------------------------------------------+
   | Common Infrastructure for  | http://esmci.github.io/cime/versions/ufs_release_v1.1/html/index.html           |
   | Modeling the Earth         |                                                                                 |
   +----------------------------+---------------------------------------------------------------------------------+
   | Unified Post Processor     | https://upp.readthedocs.io/en/ufs-v1.1.0                                        |
   +----------------------------+---------------------------------------------------------------------------------+

..
   COMMENT: Update version numbers/links!

The UFS community is encouraged to contribute to the development effort of all related
utilities, model code, and infrastructure. Users can post issues in the related GitHub repositories to report bugs or to announce upcoming contributions to the code base. For code to be accepted in the authoritative repositories, users must follow the code management rules of each UFS component repository, which are outlined in the respective User's Guides listed in :numref:`Table %s <list_of_documentation>`. In particular, innovations involving the UFS Weather Model need to be tested using the regression tests described in its User’s Guide. These tests are part of the
official NOAA policy on accepting innovations into its code base, whereas the MRW App end-to-end tests
are meant as a sanity check for users.

..
   COMMENT: Revise this to better reflect WE2E test purposes. 

Future Direction
================
Users can expect to see incremental improvements and additional capabilities in upcoming releases of the MRW Application to enhance research opportunities and support operational forecast implementations. 

Planned advancements include addition of: 

   * component models for other Earth domains (such as oceans and sea ice)
   * cycled data assimilation for model initialization
   * expansion of supported platforms

..
   COMMENT: Are these up-to-date/accurate? Are any other enhancements in the works for future MRW releases? That GO-CART thing, for example?

.. bibliography:: references.bib

