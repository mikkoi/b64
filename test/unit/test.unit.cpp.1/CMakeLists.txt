include_directories(
  ${STLSoft_INCLUDE_DIRS}
  ${xTests_INCLUDE_DIRS}
)

add_executable( test.unit.cpp.1 test.unit.cpp.1.cpp )
target_link_libraries( test.unit.cpp.1 b64.core xTests::xTests )
add_test( test.unit.cpp.1 ./test.unit.cpp.1 )
add_custom_target( tests DEPENDS test.unit.cpp.1 )

