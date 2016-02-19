include_directories(
  ${STLSoft_INCLUDE_DIRS}
  ${xTests_INCLUDE_DIRS}
)

add_executable( test.unit.c.1 test.unit.c.1.c )
target_link_libraries( test.unit.c.1 b64.core xTests::xTests )
add_test( test.unit.c.1 ./test.unit.c.1 )
add_custom_target( tests DEPENDS test.unit.c.1 )

