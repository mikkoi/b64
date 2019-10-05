include_directories(
  ${STLSoft_INCLUDE_DIRS}
  ${xTests_INCLUDE_DIRS}
)

add_executable( test.unit.cpp.1 test.unit.cpp.1.cpp )
target_link_libraries( test.unit.cpp.1 b64 xTests::xTests )
add_test(NAME test.unit.cpp.1 COMMAND ./test.unit.cpp.1 )

