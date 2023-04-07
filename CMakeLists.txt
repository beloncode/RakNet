cmake_minimum_required(VERSION 3.18)

project(RakNet)
unset(CMAKE_CXX_FLAGS )
unset(CMAKE_CXX_LINK_FLAGS )

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if(UNIX)
	add_compile_options(-w)
endif()

if(NOT APPLE)

if(CMAKE_SIZEOF_VOID_P MATCHES "4")
  set(HAVE_64_BIT 0)
else(CMAKE_SIZEOF_VOID_P MATCHES "4")
  set(HAVE_64_BIT 1)
endif(CMAKE_SIZEOF_VOID_P MATCHES "4")
endif(NOT APPLE)

IF (WIN32 AND NOT UNIX)
	set(PROGRAMFILESX86 $ENV{PROGRAMFILES})
	string(REPLACE "\\" "/" PROGRAMFILESX86 ${PROGRAMFILESX86})
ENDIF(WIN32 AND NOT UNIX)

IF (WIN32 AND NOT UNIX)
	set(RAKNET_LIBRARY_LIBS ws2_32.lib)
ELSE(WIN32 AND NOT UNIX)
	set(RAKNET_LIBRARY_LIBS pthread)
ENDIF(WIN32 AND NOT UNIX)

# Options
option(RAKNET_ENABLE_SAMPLES "Generate RakNet sample projects if true."     TRUE)
option(RAKNET_ENABLE_DLL     "Generate the DLL project if true."            TRUE)
option(RAKNET_ENABLE_STATIC  "Generate the static library project if true." FALSE)
option(RAKNET_GENERATE_INCLUDE_ONLY_DIR "Setup a include/RakNet/ directory in which all the headers are copied." FALSE)

set(RAKNETHEADERFILES ${RakNet_SOURCE_DIR}/Source)

if(RAKNET_GENERATE_INCLUDE_ONLY_DIR)
	set(RAKNET_INCLUDE_ONLY_DIR ${RakNet_SOURCE_DIR}/include) # this will be visible by client code
	set(RAKNET_NAMED_INCLUDE_ONLY_DIR ${RAKNET_INCLUDE_ONLY_DIR}/RakNet)

	file(MAKE_DIRECTORY ${RAKNET_NAMED_INCLUDE_ONLY_DIR})
	file(COPY ${RAKNETHEADERFILES}/ DESTINATION ${RAKNET_NAMED_INCLUDE_ONLY_DIR} FILES_MATCHING PATTERN "*.h")
endif()

set(RAKNET_INCLUDE_DIRS ${RAKNETHEADERFILES} ${RAKNET_INCLUDE_ONLY_DIR})

include(CmakeIncludes/CmakeMacros.txt)

FIXLINKOPTIONS()
FIXCOMPILEOPTIONS()

add_subdirectory(Lib)
set(RAKNET_COMMON_LIBS RakNetLibStatic)