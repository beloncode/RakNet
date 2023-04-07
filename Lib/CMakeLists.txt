set(RAKNET_INTERNAL_INCLUDE_DIRS 
	${RakNet_SOURCE_DIR}/Source 
	${RakNet_SOURCE_DIR}/DependentExtensions/openssl-1.0.0d/include 
	${RakNet_SOURCE_DIR}/DependentExtensions
)

FILE(GLOB RAKNET_HEADER_SRCS ${RakNet_SOURCE_DIR}/Source/*.h)
FILE(GLOB RAKNET_CPP_SRCS ${RakNet_SOURCE_DIR}/Source/*.cpp)

if(UNIX)
	add_compile_options(-fpermissive)
	find_package(Threads REQUIRED)
elseif()
endif()

if(RAKNET_ENABLE_STATIC)
	add_subdirectory(Static)
endif()

if(RAKNET_ENABLE_DLL)
	add_subdirectory(Shared)
endif()

target_link_libraries(RakNet Threads::Threads)
