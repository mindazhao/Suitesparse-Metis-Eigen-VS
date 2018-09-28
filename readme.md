# suitesparse-eigen-vs-installation
This is a document helping you for installing suitesparse on microsoft visual studio
===============================================================================
1 Installation of eigen 
------------------
The eigen is intalled with the version earlier than eigen-3.3 at http://eigen.tuxfamily.org/index.php?title=Special%3AAllPages&from=&to=&namespace=100, if version is later than 3.3(include 3.3) Some errors may
occur when executing the project.

2 Installation of suitesparse-metis-for-windows
---------------------------------------------
Some version of suitesparse-metis-for-windows can be got at https://github.com/jlblancoc/suitesparse-metis-for-windows/releases
Here we select the version 1.3.0

* install [cmake](https://cmake.org/)
* download the suitesparse-metis-for-windows to an certain path which is defined as SP_ROOT
* open SP_ROOT/metis/CMakeLists.txt, and add the command line "cmake_policy(SET CMP0022 NEW)" below "project(METIS)" 

```
cmake_minimum_required(VERSION 2.8)
project(METIS)
cmake_policy(SET CMP0022 NEW)
set(GKLIB_PATH "GKlib" CACHE PATH "path to GKlib")
set(SHARED FALSE CACHE BOOL "build a shared library")
 
if(MSVC)
  set(METIS_INSTALL FALSE)
else()
  set(METIS_INSTALL TRUE)
endif()
 
# Configure libmetis library.
if(SHARED)
  set(METIS_LIBRARY_TYPE SHARED)
else()
  set(METIS_LIBRARY_TYPE STATIC)
endif(SHARED)
 
include(${GKLIB_PATH}/GKlibSystem.cmake)
# Add include directories.
include_directories(${GKLIB_PATH})
include_directories(include)
# Recursively look for CMakeLists.txt in subdirs.
add_subdirectory("include")
add_subdirectory("libmetis")
add_subdirectory("programs")

```

