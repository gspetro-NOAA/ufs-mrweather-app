.. _Glossary:

*************************
Glossary
*************************

.. glossary::

   CCPP
      The `Common Community Physics Package <https://dtcenter.org/community-code/common-community-physics-package-ccpp>`_ is a forecast-model agnostic, vetted collection of codes containing atmospheric physical parameterizations and suites of parameterizations for use in Numerical Weather Prediction (NWP) along with a framework that connects the physics to the host forecast model.

   chgres_cube
       The preprocessing software used to create initial condition files to “coldstart” the forecast
       model. The initial conditions are created from either GFS GRIB2 or NEMSIO data.

   CIME
      The Common Infrastructure for Modeling the Earth (CIME - pronounced “SEAM”) provides a Case
      Control System for configuring, compiling and executing Earth system models, data and stub model
      components, a driver and associated tools and libraries.

   Component
      A software element that has a clear function and interface. In Earth system models, components are often single portions of the Earth system (e.g. atmosphere, ocean, or land surface) that are assembled to form a whole.

   Compset
   Compsets
      A component set. It refers to a particular mix of components, along with a component-specific configuration and/or namelist settings”.

   Dynamical core
      Global atmospheric model based on fluid dynamics principles, including Euler's equations of motion.

   FMS
      The Flexible Modeling System (FMS) is a software framework for supporting the efficient
      development, construction, execution, and scientific interpretation of atmospheric,
      oceanic, and climate system models.

   FV3
      The Finite-Volume Cubed-Sphere Dynamical Core (dycore). Developed at NOAA's Geophysical 
      Fluid Dynamics Laboratory (GFDL), it is a scalable and flexible dycore capable of both 
      hydrostatic and non-hydrostatic atmospheric simulations. It is the dycore used in the 
      UFS Weather Model.

   GFS
      `Global Forecast System <https://www.ncei.noaa.gov/products/weather-climate-models/global-forecast>`_. The GFS is a National Centers for Environmental Prediction (NCEP) weather forecast model that generates data for dozens of atmospheric and land-soil variables, including temperatures, winds, precipitation, soil moisture, and atmospheric ozone concentration. The system couples four separate models (atmosphere, ocean model, land/soil model, and sea ice) that work together to accurately depict weather conditions.

   GRIB2 
      The second version of the World Meterological Organization's (WMO) standard for distributing gridded data. 

   HPC-Stack
      The `HPC-Stack <https://github.com/NOAA-EMC/hpc-stack>`__ is a repository that provides a unified, shell script-based build system for building the software stack required for numerical weather prediction (NWP) tools such as the `Unified Forecast System (UFS) <https://ufscommunity.org/>`__ and the `Joint Effort for Data assimilation Integration (JEDI) <https://jointcenterforsatellitedataassimilation-jedi-docs.readthedocs-hosted.com/en/latest/>`__ framework.

   MPI
      MPI stands for Message Passing Interface. An MPI is a standardized communication system used in parallel programming. It establishes portable and efficient syntax for the exchange of messages and data between multiple processors that are used by a single computer program. An MPI is required for high-performance computing (HPC).

   NCEP
      National Centers for Environmental Prediction, an arm of the National Weather Service.

   NCEPLIBS
      The NCEP library source code and utilities required for chgres_cube, the UFS Weather Model, and UPP.

   NCEPLIBS-external
      A collection of third-party libraries required to build NCEPLIBS, chgres_cube, the UFS Weather Model, and UPP.

   NCL
      An interpreted language designed specifically for scientific data analysis and visualization.
      More information can be found at https://www.ncl.ucar.edu.

   NEMS
      The NOAA Environmental Modeling System - a software infrastructure that supports
      NCEP/EMC’s forecast products.

   NEMSIO
      A binary format for atmospheric model output on the native gaussian grid.

   NetCDF
      A set of software libraries and machine-independent data formats that supports the creation, access, and sharing of array-oriented scientific data. 

   spack-stack
      The `spack-stack <https://github.com/NOAA-EMC/spack-stack>`__ is a collaborative effort between the NOAA Environmental Modeling Center (EMC), the UCAR Joint Center for Satellite Data Assimilation (JCSDA), and the Earth Prediction Innovation Center (EPIC). *spack-stack* is a repository that provides a system for building the software stack required for numerical weather prediction (NWP) tools such as the `Unified Forecast System (UFS) <https://ufscommunity.org/>`__ and the `Joint Effort for Data assimilation Integration (JEDI) <https://jointcenterforsatellitedataassimilation-jedi-docs.readthedocs-hosted.com/en/latest/>`__ framework. It uses the Spack package manager along with custom Spack configuration files and Python scripts to simplify installation of the libraries required to run various applications. The *spack-stack* can be installed on a range of platforms and comes pre-configured for many systems. Users can install the necessary packages for a particular application and later add the missing packages for another application without having to rebuild the entire stack.

   Stochastic physics
      A package of stochastic schemes used to represent model uncertainty: SKEB (Stochastic Kinetic Energy Backscatter), SPPT (Stochastically Perturbed Physics Tendencies), and SHUM (Specific Humidity)

   ..
      COMMENT: Need definition of the field of Stochastic physics. Then can specify that there is a stochastic physics package that does specific things. 

   Suite
      A collection of primary physics schemes and interstitial schemes that are known to work
      well together

   UFS
      A Unified Forecast System (UFS) is a community-based, coupled comprehensive Earth
      system modeling system. The UFS numerical applications span local to global domains
      and predictive time scales from sub-hourly analyses to seasonal predictions. It is
      designed to support the Weather Enterprise and to be the source system for NOAA's
      operational numerical weather prediction applications

   UPP
      The `Unified Post Processor <https://dtcenter.org/community-code/unified-post-processor-upp>`__ is software developed at :term:`NCEP` and used operationally for models maintained by NCEP. The UPP processes raw model output from a variety of :term:`NCEP`'s NWP models, including the FV3.

   Weather Enterprise
      Individuals and organizations from public, private, and academic sectors that contribute to the research, development, and production of weather forecast products; primary consumers of these weather forecast products.

   Weather Model
      A prognostic model that can be used for short- and medium-range research and
      operational forecasts. It can be an atmosphere-only model or be an atmospheric
      model coupled with one or more additional components, such as a wave or ocean model.
