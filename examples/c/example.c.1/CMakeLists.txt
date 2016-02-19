include_directories( ${STLSoft_INCLUDE_DIRS} )

add_executable( example.c.1 example.c.1.c )
target_link_libraries (example.c.1 b64.core xTests::xTests )
add_custom_target(examples DEPENDS example.c.1 )

