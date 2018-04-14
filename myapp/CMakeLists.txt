add_executable(myapp src/myapp.c)
include_directories(${CMAKE_SOURCE_DIR}/libmylib/include)
target_link_libraries(myapp mylib)

SET_TARGET_PROPERTIES(myapp PROPERTIES OUTPUT_NAME "mylib_sample")

INSTALL(TARGETS myapp RUNTIME DESTINATION bin)

