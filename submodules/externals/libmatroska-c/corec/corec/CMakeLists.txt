set(COREC_SOURCES
	array/array.c
	helpers/file/bufstream.c
	helpers/file/memstream.c
	helpers/file/streams.c
	helpers/file/tools.c
	helpers/md5/md5.c
	helpers/parser/parser2.c
	helpers/parser/strtab.c
	helpers/parser/strtypes.c
	helpers/parser/dataheap.c
	helpers/parser/buffer.c
	helpers/parser/hotkey.c
	helpers/parser/nodelookup.c
	helpers/parser/urlpart.c
	node/node.c
	node/nodetree.c
	str/str.c
)

if(CMAKE_HOST_WIN32)
	list(APPEND COREC_SOURCES
		helpers/charconvert/charconvert_win32.c
		helpers/date/date_win32.c
		helpers/file/file_win32.c
		helpers/system/ccsystem_win32.c
		multithread/multithread_win32.c
		str/str_win32.c
	)
else()
	list(APPEND COREC_SOURCES
		helpers/date/date_libc.c
		helpers/file/file_libc.c
		helpers/system/ccsystem_posix.c
		multithread/multithread_pthread.c
	)
	if(CMAKE_HOST_APPLE)
		list(APPEND COREC_SOURCES
                	helpers/charconvert/charconvert_osx.c
                	str/str_osx.c
		)
	else()
		list(APPEND COREC_SOURCES
			helpers/charconvert/charconvert_linux.c
			str/str_linux.c
		)
	endif()
endif()

if(${CONFIG_STDIO})
	list(APPEND COREC_SOURCES
		helpers/file/stream_stdio.c
	)
endif()

add_library(corec STATIC ${COREC_SOURCES})

install(TARGETS corec
	ARCHIVE DESTINATION ${CMAKE_INSTALL_PREFIX}/lib
)

install(FILES
	banned.h
	confhelper.h
	corec.h
	err.h
	helper.h
	memalloc.h
	memheap.h
	portab.h
	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec
)

install(FILES array/array.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/array)
install(FILES helpers/charconvert/charconvert.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/charconvert)
install(FILES helpers/date/date.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/date)
install(FILES helpers/file/file.h
	helpers/file/streams.h
	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/file
)
install(FILES helpers/md5/md5.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/md5)
install(FILES helpers/parser/buffer.h
	helpers/parser/dataheap.h
	helpers/parser/hotkey.h
	helpers/parser/nodelookup.h
	helpers/parser/parser.h
	helpers/parser/strtab.h
	helpers/parser/strtypes.h
	helpers/parser/urlpart.h
	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/parser
)
install(FILES helpers/system/ccsystem.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/helpers/system)
install(FILES multithread/multithread.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/multithread)
install(FILES node/node.h
	node/node_internal.h
	node/nodebase.h
	node/nodetools.h
	node/nodetree.h
	DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/node
)
install(FILES str/str.h DESTINATION ${CMAKE_INSTALL_PREFIX}/include/corec/str)
