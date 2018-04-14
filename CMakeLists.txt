cmake_minimum_required(VERSION 2.8)

# Prohibit in-source build
IF("${CMAKE_SOURCE_DIR}" STREQUAL "${CMAKE_BINARY_DIR}")
  message(FATAL_ERROR "In-source build is not supported. Please, use an empty directory for building the project.")
ENDIF()


cmake_policy(SET CMP0048 NEW)

project(libmylib VERSION 0.0.1)

#set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
set(CMAKE_BUILD_TYPE Debug)

include(CheckCSourceCompiles)
include(CheckCSourceRuns)
include(CheckFunctionExists)
include(CheckIncludeFile)
include(CheckIncludeFiles)
include(CheckLibraryExists)
include(CheckSymbolExists)
include(CheckTypeSize)

# Check headers
set(INCLUDES "")
macro(ck_check_include_file header var)
  check_include_files("${INCLUDES};${header}" ${var})
  if(${var})
    set(INCLUDES ${INCLUDES} ${header})
  endif(${var})
endmacro(ck_check_include_file)

get_property(LIB64 GLOBAL PROPERTY FIND_LIBRARY_USE_LIB64_PATHS)

if ("${LIB64}" STREQUAL "TRUE")
	set(LIBSUFFIX 64)
else()
	set(LIBSUFFIX "")
endif()

set(INSTALL_LIB_DIR     lib${LIBSUFFIX} CACHE PATH "Installation directory for libraries")
mark_as_advanced(INSTALL_LIB_DIR)

add_subdirectory(libmylib ${CMAKE_CURRENT_BINARY_DIR}/libmylib)
add_subdirectory(myapp ${CMAKE_CURRENT_BINARY_DIR}/myapp)
#add_subdirectory(tests ${CMAKE_CURRENT_BINARY_DIR}/tests)

set(CMAKE_VERBOSE_MAKEFILE 1)


# testcase
#enable_testing()
#add_test(NAME check_hello COMMAND check_hello)
#add_test(NAME check_test2 COMMAND check_test2)

# 'make package' creates tar.bz2 binary package
set(CPACK_GENERATOR "TBZ2")
set(CPACK_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${PROJECT_VERSION}-bin")

# 'make package_source' create source package
set(CPACK_SOURCE_GENERATOR "TBZ2")

set(CPACK_SOURCE_PACKAGE_FILE_NAME "${CMAKE_PROJECT_NAME}-${PROJECT_VERSION}")
set(CPACK_SOURCE_IGNORE_FILES "/_build/;/build/;/config/;/.git/;/.svn/;${CPACK_SOURCE_IGNORE_FILES}")
include(CPack)

configure_file(
	${CMAKE_CURRENT_SOURCE_DIR}/libmylib.pc.in
	${CMAKE_CURRENT_BINARY_DIR}/libmylib.pc
	@ONLY
)

install(FILES ${CMAKE_CURRENT_BINARY_DIR}/libmylib.pc
  DESTINATION lib/pkgconfig)

