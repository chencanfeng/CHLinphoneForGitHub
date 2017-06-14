cmake_minimum_required(VERSION 3.0)

project(matroska2
	VERSION 0.22.1
	LANGUAGES C
)

option(CONFIG_EBML_WRITING "Enable EBML file writing support" ON)
option(CONFIG_EBML_UNICODE "Enable Unicode support" ON)
option(CONFIG_DEBUGCHECKS "Enable internal checks" ON)
option(CONFIG_STDIO "Use stdio.h for writing" ON)
option(CONFIG_FILEPOS_64 "Encode filepos_t on 64bits" ON)
option(CONFIG_DEBUG_LEAKS "Enable internal menory leaks checking support" OFF)

configure_file(config.h.cmake ${CMAKE_BINARY_DIR}/config.h)

include_directories(${CMAKE_BINARY_DIR} ./corec)

if(NOT ${CMAKE_HOST_SYSTEM_NAME} STREQUAL Windows)
	add_compile_options(-fPIC)
endif()

add_compile_options(-w)

add_subdirectory(corec/corec)
add_subdirectory(libebml2)
add_subdirectory(libmatroska2)

install(FILES ${CMAKE_BINARY_DIR}/config.h
	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
	${CMAKE_BINARY_DIR}/Matroska2ConfigVersion.cmake
	VERSION ${MATROSKA2_VERSION}
	COMPATIBILITY AnyNewerVersion
)

configure_file(Matroska2Config.cmake.in
	${CMAKE_BINARY_DIR}/Matroska2Config.cmake
	@ONLY
)

install(FILES
	${CMAKE_BINARY_DIR}/Matroska2ConfigVersion.cmake
	${CMAKE_BINARY_DIR}/Matroska2Config.cmake
	DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/cmake/Matroska2
)
