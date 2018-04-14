add_library(mylib STATIC
	src/mylib.c
	include/mylib.h
)

include_directories(${CMAKE_SOURCE_DIR}/libmylib/include)

#SET_TARGET_PROPERTIES(hellolib PROPERTIES OUTPUT_NAME "hello")
#SET_TARGET_PROPERTIES(hellolib PROPERTIES PREFIX "lib")

INSTALL(TARGETS mylib ARCHIVE DESTINATION ${INSTALL_LIB_DIR})
INSTALL(FILES include/mylib.h DESTINATION include)

