cmake_minimum_required (VERSION 2.8)

project ( trick_decoder )

# This option allows you to choose if you want to build a DLL or EXE:
option(BUILD_AS_DLL "Build the project as a DLL" OFF)

set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")

# include libpeconv headers:
include_directories ( ${PECONV_DIR}/include )

set (srcs
	trick_bot.cpp
)

# general headers - they will be used for both EXE and DLL:
set (hdrs
	include/types.h
	trick_bot.h
)

set (rsrc
	resource.rc
	resource.h
)

# DLL-specific headers - they will be included only in the DLL build:
set (dll_hdrs
#put your headers here
)

# Choose to build the DLL or EXE

if(BUILD_AS_DLL)
	include_directories (
		include
	)
	set (dll_hdrs
		${hdrs}
		include/trick_decoder_api.h
	)
	add_library ( ${PROJECT_NAME} SHARED ${dll_hdrs} ${srcs} ${rsrc} dll_main.cpp main.def)
else()
	add_executable ( ${PROJECT_NAME} ${exe_hdrs} ${srcs} ${rsrc} main.cpp )
endif()

# link with libpeconv.lib
target_link_libraries ( ${PROJECT_NAME} ${PECONV_LIB} )

#dependencies:
add_dependencies( ${PROJECT_NAME} libpeconv )
