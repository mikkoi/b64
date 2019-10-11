find_package(STLSoft 1.10 REQUIRED)
add_executable(example.cpp.2 example.cpp.2.cpp)
target_link_libraries(example.cpp.2 b64 STLSoft::STLSoft)

