project(spinglass_master)

#--------------------------------------------------
# Setup some convenience flags depending on machine
#--------------------------------------------------
SET(COMPUTERNAME $ENV{COMPUTERNAME})
IF (COMPUTERNAME)
  SET(HOSTNAME $ENV{COMPUTERNAME})
ELSE(COMPUTERNAME)
  SET(HOSTNAME $ENV{HOSTNAME})
ENDIF (COMPUTERNAME)

#--------------------------------------------------
# cmake policies
# see http://www.cmake.org/cmake/help/v2.8.12/cmake.html#section_Policies
#--------------------------------------------------
foreach(p
    CMP0003 CMP0017 CMP0023 CMP0042
    )
  if(POLICY ${p})
    cmake_policy(SET ${p} NEW)
  endif()
endforeach()

#--------------------------------------------------
# cmake old/new compatibility
#--------------------------------------------------
cmake_minimum_required(VERSION 2.8)

#--------------------------------------------------
# load script for checking out projects from git
#--------------------------------------------------
include(${CMAKE_CURRENT_SOURCE_DIR}/CMake/GitExternal.cmake)

#--------------------------------------------------
# load add_subproject macro 
#--------------------------------------------------
include(${CMAKE_CURRENT_SOURCE_DIR}/CMake/SubProject.cmake)

#--------------------------------------------------
# checkout projects we want to build together
#--------------------------------------------------
Git_External(
  ${CMAKE_CURRENT_SOURCE_DIR}/hpx
  https://github.com/biddisco/hpx.git
  master
  NO_UPDATE
  VERBOSE
)
Git_External(
  ${CMAKE_CURRENT_SOURCE_DIR}/spin_glass_solver
  git@github.com:Delaya/spin_glass_solver.git
  master
  NO_UPDATE
  VERBOSE
)

#--------------------------------------------------
# Testing : Include the standard testing module
#--------------------------------------------------
include(CTest)
include(Dart)

#--------------------------------------------------
# Setup Output directories
#--------------------------------------------------
SET (CMAKE_RUNTIME_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/bin CACHE PATH "Single Directory for all Executables."
)
SET (CMAKE_LIBRARY_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/bin CACHE PATH "Single Directory for all Libraries"
)
SET (CMAKE_ARCHIVE_OUTPUT_DIRECTORY
    ${PROJECT_BINARY_DIR}/bin CACHE PATH "Single Directory for all static libraries."
)

#--------------------------------------------------
# Custom setting for me to save some time, 
#--------------------------------------------------
set(BUILD_TESTING OFF CACHE BOOL "" FORCE)
set(HPX_WITH_TESTING OFF CACHE BOOL "" FORCE)
set(HPX_WITH_TESTS OFF CACHE BOOL "" FORCE)
set(HPX_WITH_EXAMPLES OFF CACHE BOOL "")

#set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} "-std=c++11")
#set(CMAKE_FIND_DEBUG_MODE 1)

#--------------------------------------------------
# Add the subprojects to our project
# args are : project name in subproject, path from here
#--------------------------------------------------
add_subproject(HPX               hpx)
add_subproject(spin_glass_solver spin_glass_solver)
#add_subproject(exa_2015 the_constructor)
