include_directories( ${STLSoft_INCLUDE_DIRS} )

add_executable( example.c.2 example.c.2.c )
target_link_libraries (example.c.2 b64.core )
add_custom_target(examples DEPENDS example.c.2 )

