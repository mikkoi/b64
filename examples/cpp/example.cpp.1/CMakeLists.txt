find_package(STLSoft 1.10 REQUIRED)
add_executable(example.cpp.1 example.cpp.1.cpp)
target_link_libraries(example.cpp.1 b64 STLSoft::STLSoft)

