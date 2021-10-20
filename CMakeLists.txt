cmake_minimum_required(VERSION 3.12)

project(lua-rtmidi)

if(WIN32)
	set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MD")
	set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MDd")
	add_compile_definitions(_CRT_SECURE_NO_WARNINGS /permissive- /Zc:__cplusplus)
else()
	add_compile_options(-fvisibility=hidden $<$<COMPILE_LANGUAGE:CXX>:-fvisibility-inlines-hidden> -std=c++11)
endif()

add_subdirectory(ext)
include_directories(/usr/local/opt/luajit/include/luajit-2.0)
link_directories(/usr/local/opt/luajit/lib)

set(LUA_RTMIDI_SRCS
	src/luartmidi.cpp
	src/rtmidi.cpp
	src/rtmidiin.cpp
	src/rtmidiout.cpp
	src/rtmidierror.cpp
	src/channelout.cpp
)

add_library(lua-rtmidilove SHARED ${LUA_RTMIDI_SRCS})
target_compile_definitions(lua-rtmidilove PRIVATE LUA_RTMIDI_DLL LUA_RTMIDI_DLL_EXPORTS)
target_link_libraries(lua-rtmidilove PRIVATE luajit rtmidi)
