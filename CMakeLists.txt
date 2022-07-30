################################################
# ZeroMQ Scratchpad programs
################################################
cmake_minimum_required( VERSION 3.24 )
project( zeromq-scratchpad LANGUAGES C CXX )

# Force overwrite install prefix path
set( CMAKE_INSTALL_PREFIX ${CMAKE_SOURCE_DIR}/build
     CACHE PATH " " FORCE )

# Use folders in IDE to organize targets
set_property( GLOBAL PROPERTY USE_FOLDERS ON )

################################################
# Build dependencies as external projects
################################################
include( ExternalProject )

ExternalProject_Add( libzmq
                     SOURCE_DIR ${CMAKE_SOURCE_DIR}/dependencies/libzmq
                     INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/external/libzmq
                     CMAKE_ARGS
                     -D CMAKE_INSTALL_PREFIX=<INSTALL_DIR>
                     -D BUILD_SHARED=TRUE
                     -D BUILD_STATIC=TRUE
)
set_target_properties( libzmq PROPERTIES FOLDER ExternalProjectTarget )

ExternalProject_Add( czmq
                     SOURCE_DIR ${CMAKE_SOURCE_DIR}/dependencies/czmq
                     INSTALL_DIR ${CMAKE_INSTALL_PREFIX}/external/czmq
                     DEPENDS libzmq
                     CMAKE_ARGS
                     -D CMAKE_INSTALL_PREFIX=<INSTALL_DIR>
                     -D libzmq_INCLUDE_DIRS=${CMAKE_INSTALL_PREFIX}/external/libzmq/include
                     -D libzmq_LIBRARY_DEBUG=${CMAKE_INSTALL_PREFIX}/external/libzmq/lib/libzmq-v143-mt-gd-4_3_5.lib
                     -D libzmq_LIBRARY_RELEASE=${CMAKE_INSTALL_PREFIX}/external/libzmq/lib/libzmq-v143-mt-sgd-4_3_5.lib
)
set_target_properties( czmq PROPERTIES FOLDER ExternalProjectTarget )

################################################
#
################################################
