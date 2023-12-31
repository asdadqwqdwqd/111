#********************************************************************************
#* Copyright (C) 2017-2020 German Aerospace Center (DLR). 
#* Eclipse ADORe, Automated Driving Open Research https://eclipse.org/adore
#*
#* This program and the accompanying materials are made available under the 
#* terms of the Eclipse Public License 2.0 which is available at
#* http://www.eclipse.org/legal/epl-2.0.
#*
#* SPDX-License-Identifier: EPL-2.0 
#*
#* Contributors: 
#*   Daniel Heß
#*   Stephan Lapoehn
#*   Thomas Lobig
#*   Andrew Koerner
#********************************************************************************
cmake_minimum_required(VERSION 3.13)
project(sumo_if_ros)

add_library(${PROJECT_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
set(PACKAGE_VENDOR "DLR TS-KOS-SAI")
set(PACKAGE_DESCRIPTION "Automated Driving Open Research: sumo interface libraries")
set(PACKAGE_MAINTAINER "DLR TS-KOS-SAI")
set(PACKAGE_MAINTAINER_EMAIL "ts-kos-sai@dlr.de")

include(GNUInstallDirs)
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include(Packing)

set_directory_properties(PROPERTIES EXCLUDE_FROM_ALL NO)

list(APPEND CMAKE_PREFIX_PATH "$ENV{CMAKE_PREFIX_PATH}")

set(SUMO_CMAKE_INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../sumo/build/install")
list(APPEND CMAKE_PREFIX_PATH "${SUMO_CMAKE_INSTALL_PATH}")

set(ADORE_V2X_SIM_CMAKE_INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../adore_v2x_sim/adore_v2x_sim/build/install")
list(APPEND CMAKE_PREFIX_PATH "${ADORE_V2X_SIM_CMAKE_INSTALL_PATH}")

set(ADORE_IF_ROS_MSG_CMAKE_INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../adore_if_ros_msg/adore_if_ros_msg/build/install")
list(APPEND CMAKE_PREFIX_PATH "${ADORE_IF_ROS_MSG_CMAKE_INSTALL_PATH}")

set(V2X_IF_ROS_MSG_CMAKE_INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../v2x_if_ros_msg/v2x_if_ros_msg/build/install")
list(APPEND CMAKE_PREFIX_PATH "${V2X_IF_ROS_MSG_CMAKE_INSTALL_PATH}")

set(COORDINATE_CONVERSION_CMAKE_INSTALL_PATH "${CMAKE_CURRENT_SOURCE_DIR}/../../coordinate_conversion/coordinate_conversion/build/install")
list(APPEND CMAKE_PREFIX_PATH "${COORDINATE_CONVERSION_CMAKE_INSTALL_PATH}")

find_package(adore_if_ros_msg CONFIG REQUIRED)
#find_package(SUMO CONFIG REQUIRED)
#find_package(v2x_if_ros_msg CONFIG REQUIRED)# does not work because v2x_if_ros_msg is poorly packaged
find_package(coordinate_conversion CONFIG REQUIRED)
find_package(adore_v2x_sim CONFIG REQUIRED)

# add_library(sumo::sumo SHARED IMPORTED GLOBAL)
# set_property(TARGET sumo::sumo PROPERTY IMPORTED_LOCATION 
#               ${CMAKE_CURRENT_SOURCE_DIR}/../../sumo/build/install/lib/libsumocpp.so
#             )

find_library(
    sumocpp_library REQUIRED
    NAMES sumocpp
    HINTS "${SUMO_CMAKE_INSTALL_PATH}/lib"
)
include_directories(${PROJECT_NAME} PRIVATE ${SUMO_CMAKE_INSTALL_PATH}/include)

## Compile as C++11, supported in ROS Kinetic and newer
# add_compile_options(-std=c++11)

## Find catkin macros and libraries
## if COMPONENTS list like find_package(catkin REQUIRED COMPONENTS xyz)
## is used, also find other catkin packages
find_package(catkin REQUIRED COMPONENTS
  roscpp
  rospy
  std_msgs
  tf
  adore_if_ros_msg
  dsrc_v2_mapem_pdu_descriptions
  dsrc_v2_spatem_pdu_descriptions
  its_container_v2_its_container
  coordinate_conversion
  adore_v2x_sim
)

## System dependencies are found with CMake's conventions
# find_package(Boost REQUIRED COMPONENTS system)


## Uncomment this if the package has a setup.py. This macro ensures
## modules and global scripts declared therein get installed
## See http://ros.org/doc/api/catkin/html/user_guide/setup_dot_py.html
# catkin_python_setup()

################################################
## Declare ROS messages, services and actions ##
################################################

## To declare and build messages, services or actions from within this
## package, follow these steps:
## * Let MSG_DEP_SET be the set of packages whose message types you use in
##   your messages/services/actions (e.g. std_msgs, actionlib_msgs, ...).
## * In the file package.xml:
##   * add a build_depend tag for "message_generation"
##   * add a build_depend and a exec_depend tag for each package in MSG_DEP_SET
##   * If MSG_DEP_SET isn't empty the following dependency has been pulled in
##     but can be declared for certainty nonetheless:
##     * add a exec_depend tag for "message_runtime"
## * In this file (CMakeLists.txt):
##   * add "message_generation" and every package in MSG_DEP_SET to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * add "message_runtime" and every package in MSG_DEP_SET to
##     catkin_package(CATKIN_DEPENDS ...)
##   * uncomment the add_*_files sections below as needed
##     and list every .msg/.srv/.action file to be processed
##   * uncomment the generate_messages entry below
##   * add every package in MSG_DEP_SET to generate_messages(DEPENDENCIES ...)

## Generate messages in the 'msg' folder
# add_message_files(
#  FILES
#   Message2.msg
# )

## Generate services in the 'srv' folder
# add_service_files(
#   FILES
#   Service1.srv
#   Service2.srv
# )

## Generate actions in the 'action' folder
# add_action_files(
#   FILES
#   Action1.action
#   Action2.action
# )

## Generate added messages and services with any dependencies listed here
#  generate_messages(
#    DEPENDENCIES
#  )

################################################
## Declare ROS dynamic reconfigure parameters ##
################################################

## To declare and build dynamic reconfigure parameters within this
## package, follow these steps:
## * In the file package.xml:
##   * add a build_depend and a exec_depend tag for "dynamic_reconfigure"
## * In this file (CMakeLists.txt):
##   * add "dynamic_reconfigure" to
##     find_package(catkin REQUIRED COMPONENTS ...)
##   * uncomment the "generate_dynamic_reconfigure_options" section below
##     and list every .cfg file to be processed

## Generate dynamic reconfigure parameters in the 'cfg' folder
# generate_dynamic_reconfigure_options(
#   cfg/DynReconf1.cfg
#   cfg/DynReconf2.cfg
# )

###################################
## catkin specific configuration ##
###################################
## The catkin_package macro generates cmake config files for your package
## Declare things to be passed to dependent projects
## INCLUDE_DIRS: uncomment this if your package contains header files
## LIBRARIES: libraries you create in this project that dependent projects also need
## CATKIN_DEPENDS: catkin_packages dependent projects also need
## DEPENDS: system dependencies of this project that dependent projects also need
catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES sumo_if_ros
#  CATKIN_DEPENDS roscpp rospy std_msgs
#  DEPENDS system_lib
  CATKIN_DEPENDS 
    adore_if_ros_msg 
    v2x_if_ros_msg 
    adore_v2x_sim
)

###########
## Build ##
###########

## Specify additional locations of header files
## Your package locations should be listed before other locations
# include_directories(
#   include
#   ${sumo_src_dir}/utils/traci
#   ${sumo_src_dir}/foreign/tcpip
#   ${sumo_src_dir}/traci-server
#   ${sumo_src_dir}/libsumo
#   ${catkin_INCLUDE_DIRS}
# )

## Declare a C++ library
# add_library(${PROJECT_NAME}
#   src/${PROJECT_NAME}/sumo_if_ros.cpp
# )

## Add cmake target dependencies of the library
## as an example, code may need to be generated before libraries
## either from message generation or dynamic reconfigure
# add_dependencies(${PROJECT_NAME} ${${PROJECT_NAME}_EXPORTED_TARGETS} ${catkin_EXPORTED_TARGETS})

## Declare a C++ executable
## With catkin_make all packages are built within a single CMake context
## The recommended prefix ensures that target names across packages don't collide
add_executable(${PROJECT_NAME}_node )
target_sources(
  ${PROJECT_NAME}_node  PRIVATE 
  src/sumotraffic2ros.cpp
  src/sumotls2ros.cpp
)
target_include_directories(
  ${PROJECT_NAME}_node  PRIVATE include
  ${sumo_src_dir}
  ${catkin_INCLUDE_DIRS}
)
set_target_properties(
    ${PROJECT_NAME}_node PROPERTIES
    INSTALL_RPATH "${SUMO_CMAKE_INSTALL_PATH}/lib"
    BUILD_WITH_INSTALL_RPATH 1
)


target_link_libraries(
    ${PROJECT_NAME}_node 
    ${catkin_LIBRARIES} 
    coordinate_conversion::coordinate_conversion
    ${sumocpp_library}
)

#############
## Install ##
#############

# all install targets should use catkin DESTINATION variables
# See http://ros.org/doc/api/catkin/html/adv_user_guide/variables.html

## Mark executable scripts (Python etc.) for installation
## in contrast to setup.py, you can choose the destination
# install(PROGRAMS
#   scripts/my_python_script
#   DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )

## Mark executables for installation
## See http://docs.ros.org/melodic/api/catkin/html/howto/format1/building_executables.html
# install(TARGETS ${PROJECT_NAME}_node
#   RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
# )
install(TARGETS
            ${PROJECT_NAME}
            ${PROJECT_NAME}_node
        EXPORT ${PROJECT_NAME}_Targets
        ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
        LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
        RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})

include(CMakePackageConfigHelpers)

configure_package_config_file(
  "${PROJECT_SOURCE_DIR}/cmake/LibraryConfig.cmake.in"
  "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
  INSTALL_DESTINATION
  ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake)

install(EXPORT ${PROJECT_NAME}_Targets
        COMPONENT ${PROJECT_NAME}
        FILE ${PROJECT_NAME}Targets.cmake
        NAMESPACE ${PROJECT_NAME}::
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake
)

install(FILES "${PROJECT_BINARY_DIR}/${PROJECT_NAME}Config.cmake"
              # "${PROJECT_BINARY_DIR}/${PROJECT_NAME}ConfigVersion.cmake"
        DESTINATION ${CMAKE_INSTALL_DATAROOTDIR}/${PROJECT_NAME}/cmake
        COMPONENT ${PROJECT_NAME}
)

install(DIRECTORY ${PROJECT_SOURCE_DIR}/build/install/include DESTINATION .) 

