#-------------------------------------------------------------------------------------------
# Copyright (C) Electronic Arts Inc.  All rights reserved.
#-------------------------------------------------------------------------------------------

#-------------------------------------------------------------------------------------------
# CMake info
#-------------------------------------------------------------------------------------------
cmake_minimum_required(VERSION 3.11...3.31)
project(EABaseTest CXX)
include(CTest)
include(FetchContent)

#-------------------------------------------------------------------------------------------
# Defines
#-------------------------------------------------------------------------------------------
add_definitions(-D_CRT_SECURE_NO_WARNINGS)
add_definitions(-D_SCL_SECURE_NO_WARNINGS)
add_definitions(-D_CHAR16T)

#-------------------------------------------------------------------------------------------
# Pull in EASTL so we can use it for the CommonCppFlags
#-------------------------------------------------------------------------------------------
if(NOT TARGET EASTL)
  FetchContent_Declare(
    EASTL
    GIT_REPOSITORY https://github.com/electronicarts/EASTL.git
    GIT_TAG 3.21.23) # TODO: Must be updated before merging
  FetchContent_MakeAvailable(EASTL)
endif()
target_link_libraries(EABaseTest EASTL)

#-------------------------------------------------------------------------------------------
# Compiler Flags
#-------------------------------------------------------------------------------------------
set (CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_CURRENT_SOURCE_DIR}/packages/EASTL/scripts/CMake")
include(CommonCppFlags)

#-------------------------------------------------------------------------------------------
# Source files
#-------------------------------------------------------------------------------------------
file(GLOB EABASETEST_SOURCES "source/TestEABase.cpp" "source/TestEABase.h")
set(SOURCES ${EABASETEST_SOURCES})

#-------------------------------------------------------------------------------------------
# Executable definition
#-------------------------------------------------------------------------------------------
add_executable(EABaseTest ${SOURCES})

# -------------------------------------------------------------------------------------------
# Dependencies
# -------------------------------------------------------------------------------------------

if(NOT TARGET EAAssert)
  FetchContent_Declare(
    EAAssert
    GIT_REPOSITORY https://github.com/electronicarts/EAAssert.git
    GIT_TAG e5e181255de2e883dd1f987c78ccc42ac81d3bca
    GIT_SUBMODULES "" # This should be temporary until we update the cyclic
                      # submodule dependencies in EAAssert.
  )
  FetchContent_MakeAvailable(EAAssert)
endif()
target_link_libraries(EABaseTest EAAssert)

if(NOT TARGET EAStdC)
  FetchContent_Declare(
    EAStdC
    GIT_REPOSITORY https://github.com/electronicarts/EAStdC.git
    GIT_TAG fbcc34e89c63636054334888f3a5bd7ac2fd4b76
    GIT_SUBMODULES "" # This should be temporary until we update the cyclic
                      # submodule dependencies in EAStdC.
  )
  FetchContent_MakeAvailable(EAStdC)
endif()
target_link_libraries(EABaseTest EAStdC)

if(NOT TARGET EAMain)
  FetchContent_Declare(
    EAMain
    GIT_REPOSITORY https://github.com/electronicarts/EAMain.git
    GIT_TAG 24ca8bf09e6b47b860286fc2f4c832f4009273d1
    GIT_SUBMODULES "" # This should be temporary until we update the cyclic
                      # submodule dependencies in EAMain.
  )
  FetchContent_MakeAvailable(EAMain)
endif()
target_link_libraries(EABaseTest EAMain)

if(NOT TARGET EATest)
  FetchContent_Declare(
    EATest
    GIT_REPOSITORY https://github.com/electronicarts/EATest.git
    GIT_TAG a59b372fc9cba517283ad6d060d2ab96e0ba34ac
    GIT_SUBMODULES "" # This should be temporary until we update the cyclic
                      # submodule dependencies in EATest.
  )
  FetchContent_MakeAvailable(EATest)
endif()
target_link_libraries(EABaseTest EATest)

if(NOT TARGET EAThread)
  FetchContent_Declare(
    EAThread
    GIT_REPOSITORY https://github.com/electronicarts/EAThread.git
    GIT_TAG f3c6c54d19699639a5edcf5237ea8b71aca6842c
    GIT_SUBMODULES "" # This should be temporary until we update the cyclic
                      # submodule dependencies in EAThread.
  )
  FetchContent_MakeAvailable(EAThread)
endif()
target_link_libraries(EABaseTest EAThread)

set(THREADS_PREFER_PTHREAD_FLAG ON)
find_package(Threads REQUIRED)

if((NOT APPLE) AND (NOT WIN32))
    target_link_libraries(EABaseTest ${EASTLTest_Libraries} Threads::Threads rt)
else()
    target_link_libraries(EABaseTest ${EASTLTest_Libraries} Threads::Threads)
endif()

#-------------------------------------------------------------------------------------------
# Run Unit tests and verify the results.
#-------------------------------------------------------------------------------------------
add_test(EABaseTestRuns EABaseTest)
set_tests_properties (EABaseTestRuns PROPERTIES PASS_REGULAR_EXPRESSION "RETURNCODE=0")

