cmake_minimum_required(VERSION 2.4.6)
include($ENV{ROS_ROOT}/core/rosbuild/rosbuild.cmake)

# Set the build type.  Options are:
#  Coverage       : w/ debug symbols, w/o optimization, w/ code-coverage
#  Debug          : w/ debug symbols, w/o optimization
#  Release        : w/o debug symbols, w/ optimization
#  RelWithDebInfo : w/ debug symbols, w/ optimization
#  MinSizeRel     : w/o debug symbols, w/ optimization, stripped binaries
#set(ROS_BUILD_TYPE RelWithDebInfo)

rosbuild_init()

#set the default path for built executables to the "bin" directory
set(EXECUTABLE_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/bin)
#set the default path for built libraries to the "lib" directory
set(LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib)

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/vcglib-svn/vcglib)


#--------------------------------------------------------------------------------
# this command finds Qt4 libraries and sets all required variables
# note that it's Qt4, not QT4 or qt4
FIND_PACKAGE( Qt4 REQUIRED )
set(QT_USE_OPENGL TRUE)
set(QT_USE_QTOPENGL TRUE)
#--------------------------------------------------------------------------------
# QT_USE_FILE is a variable defined by FIND_PACKAGE( Qt4 ) that contains a path to
# a CMake script that sets up this directory for building with Qt4
INCLUDE( ${QT_USE_FILE} )

SET(DIRS ${QT_LIBRARY_DIRS})

#uncomment if you have defined messages
#rosbuild_genmsg()
#uncomment if you have defined services
#rosbuild_gensrv()


# chsckout  vcglib
execute_process(COMMAND cmake -E chdir ${PROJECT_SOURCE_DIR} make -f Makefile.vcglib
                RESULT_VARIABLE _make_failed)
if(_make_failed)
  message(FATAL_ERROR "Could not check out vcglib")
endif(_make_failed)


rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/ply/plylib.cpp)

rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/system/getopt.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/system/qgetopt.cpp)
#rosbuild_add_library(${PROJECT_NAME} include/vcglib/wrap/bmt/bmt.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/dae/xmldocumentmanaging.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC  include/vcglib/wrap/ply/plylib.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/gui/activecoordinateframe.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/gui/coordinateframe.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/gui/trackmode.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/gui/trackball.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/wrap/gui/rubberband.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/vcg/Eigen/src/Cholesky/CholeskyInstantiations.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/vcg/Eigen/src/QR/QrInstantiations.cpp)
rosbuild_add_library(${PROJECT_NAME} STATIC include/vcglib/vcg/Eigen/src/Core/CoreInstantiations.cpp)


#common commands for building c++ executables and libraries

#target_link_libraries(${PROJECT_NAME} another_library)
#rosbuild_add_boost_directories()
#rosbuild_link_boost(${PROJECT_NAME} thread)
#rosbuild_add_executable(example examples/example.cpp)
#target_link_libraries(example ${PROJECT_NAME})
