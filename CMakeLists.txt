cmake_minimum_required(VERSION 2.8.3)
project(libvimba)

find_package(catkin REQUIRED)

set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include(TargetArchitecture)
target_architecture(ARCH)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES VimbaCPP VimbaC
)

###########
## Build ##
###########
include_directories(include)

add_library(VimbaC SHARED IMPORTED)
add_library(VimbaCPP SHARED IMPORTED)
if("${ARCH}" STREQUAL i386)
  set_property(TARGET VimbaC PROPERTY IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/lib/x86_32bit/libVimbaC.so)
  set_property(TARGET VimbaCPP PROPERTY IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/lib/x86_32bit/libVimbaCPP.so)
elseif("${ARCH}" STREQUAL x86_64)
  set_property(TARGET VimbaC PROPERTY IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/lib/x86_64bit/libVimbaC.so)
  set_property(TARGET VimbaCPP PROPERTY IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/lib/x86_64bit/libVimbaCPP.so)
else()
  message(FATAL_ERROR "[libvimba]: Architecture ${ARCH} not suported. Exiting...")
endif()

#############
## Install ##
#############

## Mark cpp header files for installation
install(DIRECTORY include
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)

## Mark imported library for installation
get_target_property(VimbaC_LOCATION VimbaC IMPORTED_LOCATION)
get_target_property(VimbaCPP_LOCATION VimbaCPP IMPORTED_LOCATION)
install(FILES
  ${VimbaC_LOCATION}
  ${VimbaCPP_LOCATION}
  DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)
