add_library(b64 STATIC b64.c)
target_link_libraries(b64)
if(xContract_FOUND)
    target_compile_definitions(b64 PRIVATE B64_USING_XCONTRACT_)
    target_link_libraries(b64 xContract::xContract)
endif()
set_target_properties(b64 PROPERTIES
    OUTPUT_NAME "b64.core"
    PUBLIC_HEADER ${CMAKE_SOURCE_DIR}/include/b64/b64.h)
target_include_directories(b64 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_SOURCE_DIR}/include>
    $<INSTALL_INTERFACE:include>)

install(TARGETS b64
    EXPORT project-targets
    ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/b64
)

