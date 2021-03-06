CMAKE_MINIMUM_REQUIRED(VERSION 2.8)

PROJECT(Agnss)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/bin)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)

option(NOCRT "link crt static" OFF)

if (MSVC)
    #warning 4251
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /wd4819 /wd4996 ")
	
    if (NOCRT)
		set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /DWIN32 /MTd")
		set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /DWIN32 /MT")
	endif()
	
	set(CMAKE_SHARED_LINKER_FLAGS_DEBUG   "${CMAKE_SHARED_LINKER_FLAGS_DEBUG} /SAFESEH:NO")
	set(CMAKE_SHARED_LINKER_FLAGS_RELEASE "${CMAKE_SHARED_LINKER_FLAGS_RELEASE} /OPT:NOREF /OPT:ICF /SAFESEH:NO")
	
    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/bin)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG ${PROJECT_SOURCE_DIR}/lib)

    set(CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/bin)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE ${PROJECT_SOURCE_DIR}/lib)
    
    if (MSVC90)
        add_definitions(-D_BIND_TO_CURRENT_VCLIBS_VERSION=1)
    endif()
else()
	set(CMAKE_CXX_VISIBILITY_PRESET hidden)
	set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -fvisibility=hidden")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fvisibility=hidden")
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
endif()


set(PROJECT_ROOT ${PROJECT_SOURCE_DIR})
set(THIRD_PARTY ${PROJECT_ROOT}/third_party)
set(COMN_DIR ${PROJECT_ROOT}/comn)
set(ZEROMQ_DIR ${THIRD_PARTY}/zeromq-4.2.1)
set(ZMQPP_DIR ${THIRD_PARTY}/zmqpp-4.1.2)


if (CMAKE_HOST_WIN32)
    set(LIB_PLATFORM ws2_32 )
else()
    set(LIB_PLATFORM pthread rt)
endif()


ADD_SUBDIRECTORY(comn)

ADD_SUBDIRECTORY(third_party EXCLUDE_FROM_ALL)

ADD_SUBDIRECTORY(AgnssCapture)
ADD_SUBDIRECTORY(AgnssServer)

	



