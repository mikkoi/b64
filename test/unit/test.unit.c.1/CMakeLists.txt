find_package(STLSoft 1.9.131 REQUIRED)
add_executable(test.unit.c.1 test.unit.c.1.c)
target_link_libraries(test.unit.c.1 b64 xTests::xTests STLSoft::STLSoft)
add_test(NAME test.unit.c.1 COMMAND ./test.unit.c.1)

