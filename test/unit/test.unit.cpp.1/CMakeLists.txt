find_package(STLSoft 1.9.131 REQUIRED)
add_executable(test.unit.cpp.1 test.unit.cpp.1.cpp)
target_link_libraries(test.unit.cpp.1 b64 xTests::xTests STLSoft::STLSoft)
add_test(NAME test.unit.cpp.1 COMMAND ./test.unit.cpp.1)

