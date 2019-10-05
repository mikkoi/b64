cmake_minimum_required(VERSION 3.9 FATAL_ERROR)
if(POLICY CMP0074)
    cmake_policy(SET CMP0074 NEW) # Use <PackageName>_ROOT variables
endif()
project(b64
  DESCRIPTION "Very small and simple C-language library for Base-64 encoding and decoding."
  LANGUAGES C CXX)

# Directory for CMake specific extensions and source files.
set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# Set build type to Debug if this is a Git repository.
# Otherwise set to Release.
# Unless user overrides on the command line.
include(BuildType)

# Configure a project for testing with CTest/CDash.
# Activate testing (var BUILD_TESTING).
# unless user overrides on the command line.
include(CTest)

# Handle version number
set(RX_WS "[ \t\r\n]")
file(READ "${CMAKE_CURRENT_SOURCE_DIR}/include/b64/b64.h" b64_header)
string(REGEX MATCH "#define${RX_WS}+B64_VER_MAJOR${RX_WS}+([0-9]+)" MAJOR_DUMMY ${b64_header})
set(_VERSION_MAJOR ${CMAKE_MATCH_1})
string(REGEX MATCH "#define${RX_WS}+B64_VER_MINOR${RX_WS}+([0-9]+)" MINOR_DUMMY ${b64_header})
set(_VERSION_MINOR ${CMAKE_MATCH_1})
string(REGEX MATCH "#define${RX_WS}+B64_VER_REVISION${RX_WS}+([0-9]+)" PATCH_DUMMY ${b64_header})
set(_VERSION_PATCH ${CMAKE_MATCH_1})

# Set project version number here
set(PROJECT_VERSION_MAJOR ${_VERSION_MAJOR})
set(PROJECT_VERSION_MINOR ${_VERSION_MINOR})
set(PROJECT_VERSION_PATCH ${_VERSION_PATCH})
set(PROJECT_VERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}.${PROJECT_VERSION_PATCH}")

# Adhere strictly to C and C++ standards plus extensions.
set(CMAKE_C_STANDARD 90)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON) # GNU extensions and POSIX standard
set(CMAKE_CXX_STANDARD 98)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

# Assuming b64 library will always be linked statically
# due to its small size.
# User can override if necessary.
set(BUILD_SHARED_LIBS FALSE)

# #########################################################
# Preparations for installing

# Provides install directory variables as defined by the GNU Coding Standards.
include(GNUInstallDirs)

# #########################################################
# Build

find_package(STLSoft 1.9.125 REQUIRED)

# The library
add_subdirectory(src)

# Tests
if(BUILD_TESTING)
    find_package(xTests 0.18.4 QUIET)
    if(xTests_FOUND)
        add_subdirectory(test)
    else()
        message(AUTHOR_WARNING "The xTests framework not found. Cannot build tests.")
    endif()
endif()

# Examples
# add_subdirectory(examples EXCLUDE_FROM_ALL)

# !!!! set(CMAKE_INCLUDE_CURRENT_DIR ON)
# set(CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON)

# #########################################################
# Export and install the project

set(EXPORT_NAME ${PROJECT_NAME})

# Prepare a config and config-version files
include(CMakePackageConfigHelpers)
configure_package_config_file(
  ${CMAKE_CURRENT_SOURCE_DIR}/cmake/${EXPORT_NAME}-config.cmake.in
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/${EXPORT_NAME}-config.cmake
  INSTALL_DESTINATION ${LIB_INSTALL_DIR}/${EXPORT_NAME}/cmake
)
write_basic_package_version_file(
  ${CMAKE_CURRENT_BINARY_DIR}/cmake/${EXPORT_NAME}-config-version.cmake
  VERSION ${PROJECT_VERSION}
  COMPATIBILITY AnyNewerVersion
)

export(EXPORT project-targets
    FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/${EXPORT_NAME}-targets.cmake"
  NAMESPACE "{$EXPORT_NAME}::"
)

# Install to GNU type subdirs under CMAKE_INSTALL_PREFIX
install(EXPORT project-targets
  NAMESPACE "${EXPORT_NAME}::"
  FILE "${EXPORT_NAME}-targets.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORT_NAME}
)
install(
  FILES
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/${EXPORT_NAME}-config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/${EXPORT_NAME}-config-version.cmake"
  DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/${EXPORT_NAME}
)

# Store the current build directory in the CMake
# user package registry for package.
# This helps dependent projects use a package
# from the current project’s build tree,
# i.e. without installing it.
export(PACKAGE "${EXPORT_NAME}")



















# generate_export_header(b64)
# set_property(TARGET b64 PROPERTY VERSION ${PROJECT_VERSION})
# set_property(TARGET b64 PROPERTY INTERFACE_b64_MAJOR_VERSION ${PROJECT_VERSION})
# set_property(TARGET b64 APPEND PROPERTY
#   COMPATIBLE_INTERFACE_STRING b64_MAJOR_VERSION
# )
#
# install(TARGETS b64 EXPORT b64Targets
#   LIBRARY DESTINATION lib
#   ARCHIVE DESTINATION lib
#   RUNTIME DESTINATION bin
# )
# install(DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/xtests"
#   DESTINATION include
#   COMPONENT Devel
# )
#
# write_basic_package_version_file(
#   "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64ConfigVersion.cmake"
#   VERSION ${b64_VERSION}
#   COMPATIBILITY AnyNewerVersion
# )
# export(TARGETS b64
#   FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Targets.cmake"
#   NAMESPACE b64::
# )
# set(PACKAGE_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")
# configure_file(cmake/b64Config.cmake.in
#   "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Config.cmake"
#   @ONLY
# )
# export(PACKAGE b64)
#
# set(ConfigPackageLocation cmake/b64)
# install(EXPORT b64Targets
#   FILE b64Targets.cmake
#   NAMESPACE b64::
#   DESTINATION ${ConfigPackageLocation}
# )
# install(FILES
#     "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Config.cmake"
#     "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64ConfigVersion.cmake"
#   DESTINATION ${ConfigPackageLocation}
#   COMPONENT Devel
# )
#
