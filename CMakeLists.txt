cmake_minimum_required( VERSION 2.8 )
project( b64 C CXX )
enable_testing()
set( b64_VERSION 1.4.3 )

# Project include directory (all includes located here)
include_directories ("${CMAKE_CURRENT_SOURCE_DIR}/include")

find_package( STLSoft 1.9.125 REQUIRED )
find_package( xTests 0.18.4 QUIET )

add_subdirectory( src )

# Build examples: "make examples".
add_subdirectory( examples EXCLUDE_FROM_ALL )
# Build tests: "make tests". Execute tests: "make test"
if( xTests_FOUND )
  add_subdirectory( test EXCLUDE_FROM_ALL )
else( xTests_FOUND )
  message( AUTHOR_WARNING "The xTests framework not found. Cannot build tests." )
endif()

set( CMAKE_INCLUDE_CURRENT_DIR ON )
set( CMAKE_INCLUDE_CURRENT_DIR_IN_INTERFACE ON )

include( GenerateExportHeader )
include( CMakePackageConfigHelpers )

include_directories (${STLSoft_INCLUDE_DIRS})
add_library(b64 src/b64.c)
generate_export_header(b64)
set_property(TARGET b64 PROPERTY VERSION ${b64_VERSION})
set_property(TARGET b64 PROPERTY INTERFACE_b64_MAJOR_VERSION ${b64_VERSION})
set_property(TARGET b64 APPEND PROPERTY
  COMPATIBLE_INTERFACE_STRING b64_MAJOR_VERSION
)

install( TARGETS b64 EXPORT b64Targets
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib
  RUNTIME DESTINATION bin
)
install( DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/include/xtests"
  DESTINATION include
  COMPONENT Devel
)

write_basic_package_version_file(
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64ConfigVersion.cmake"
  VERSION ${b64_VERSION}
  COMPATIBILITY AnyNewerVersion
)
export(TARGETS b64
  FILE "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Targets.cmake"
  NAMESPACE b64::
)
set(PACKAGE_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")
configure_file(cmake/b64Config.cmake.in
  "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Config.cmake"
  @ONLY
)
export(PACKAGE b64)

set(ConfigPackageLocation cmake/b64)
install( EXPORT b64Targets
  FILE b64Targets.cmake
  NAMESPACE b64::
  DESTINATION ${ConfigPackageLocation}
)
install( FILES
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64Config.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/cmake/b64ConfigVersion.cmake"
  DESTINATION ${ConfigPackageLocation}
  COMPONENT Devel
)

