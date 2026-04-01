# Copyright (C) 2017-2023 Jonathan Müller and cppast contributors
# SPDX-License-Identifier: MIT
# found in the top-level directory of this distribution.

cmake_minimum_required(VERSION 3.30)
project(cppast VERSION 0.1.0)

if(CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(is_top_level_project TRUE)
else()
    set(is_top_level_project FALSE)
endif()

# options
option(CPPAST_BUILD_TEST "Whether or not to build the tests" OFF)
option(CPPAST_BUILD_EXAMPLE "Whether or not to build the examples" OFF)
option(CPPAST_BUILD_TOOL "Whether or not to build the tool" OFF)
option(CPPAST_USE_STATIC_LIBS "Force cppast and dependencies to use static libraries (excluded libclang on Windows)" OFF)

if(CPPAST_USE_STATIC_LIBS)
    set(BUILD_SHARED_LIBS OFF CACHE BOOL "" FORCE)
else()
    set(BUILD_SHARED_LIBS ON CACHE BOOL "" FORCE)
endif()

if(${CPPAST_BUILD_TEST} OR ${is_top_level_project})
    set(build_test ON)
    set(CMAKE_EXPORT_COMPILE_COMMANDS ON) # for the self integration test
else()
    set(build_test OFF)
endif()

if(${CPPAST_BUILD_EXAMPLE} OR ${is_top_level_project})
    set(build_example ON)
else()
    set(build_example OFF)
endif()

if(${CPPAST_BUILD_TOOL} OR ${is_top_level_project})
    set(build_tool ON)
else()
    set(build_tool OFF)
endif()

include(external/external.cmake)

if(build_test AND CPPAST_TEST_GCOV AND (CMAKE_CXX_COMPILER_ID STREQUAL "GNU"))
    list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")
    include(CodeCoverage)
    APPEND_COVERAGE_COMPILER_FLAGS()
endif()

add_subdirectory(src)

if(${build_test})
    enable_testing()
    add_subdirectory(test)
endif()
if(${build_example})
    add_subdirectory(example)
endif()
if(${build_tool})
    add_subdirectory(tool)
endif()
