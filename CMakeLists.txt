#-------------------------------------------------------------------------------------------
# Copyright (C) Electronic Arts Inc.  All rights reserved.
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.14)
project(EABase
    VERSION 2.09.12
    DESCRIPTION "EABase is a small set of header files that define platform-independent data types and macros."
    LANGUAGES CXX
)

#-------------------------------------------------------------------------------------------
# Options
#-------------------------------------------------------------------------------------------
option(EABASE_BUILD_TESTS "Enable generation of build files for tests" OFF)

#-------------------------------------------------------------------------------------------
# Package Tests 
#-------------------------------------------------------------------------------------------
if(EABASE_BUILD_TESTS)
    add_subdirectory(test)
endif()

#-------------------------------------------------------------------------------------------
# Defines 
#-------------------------------------------------------------------------------------------
add_definitions(-D_CHAR16T)

#-------------------------------------------------------------------------------------------
# Header only library
#-------------------------------------------------------------------------------------------
add_library(EABase INTERFACE)
add_library(EABase::EABase ALIAS EABase)
#-------------------------------------------------------------------------------------------
# Include dirs
#-------------------------------------------------------------------------------------------
target_include_directories(EABase INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/include/Common>)

#-------------------------------------------------------------------------------------------
# Installation
#-------------------------------------------------------------------------------------------
include(GNUInstallDirs)
include(CMakePackageConfigHelpers)

install(
    TARGETS EABase EXPORT EABaseTargets
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    INCLUDES DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)

install(
    EXPORT EABaseTargets
    FILE EABaseTargets.cmake
    NAMESPACE EABase::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/EABase
)

configure_package_config_file(
    ${CMAKE_CURRENT_LIST_DIR}/script/CMake/EABaseConfig.cmake.in
    ${CMAKE_CURRENT_BINARY_DIR}/EABaseConfig.cmake
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/EABase
)

write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/EABaseConfigVersion.cmake
    VERSION ${PROJECT_VERSION}
    COMPATIBILITY SameMajorVersion
)

install(
    FILES
        ${CMAKE_CURRENT_BINARY_DIR}/EABaseConfig.cmake
        ${CMAKE_CURRENT_BINARY_DIR}/EABaseConfigVersion.cmake
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/EABase
)

install(
    DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/Common/
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}
)
