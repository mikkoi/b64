find_package( STLSoft 1.10 QUIET )
if( STLSoft_FOUND )
  include_directories( ${STLSoft_INCLUDE_DIRS} )
  add_executable( example.cpp.1 example.cpp.1.cpp )
  target_link_libraries( example.cpp.1 b64.core xTests::xTests )
  add_custom_target( examples DEPENDS example.cpp.1 )
endif( STLSoft_FOUND )

