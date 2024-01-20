# CMakeList.txt: tinyhttpd 的 CMake 项目，在此处包括源代码并定义
# 项目特定的逻辑。
#
cmake_minimum_required (VERSION 3.8)

set(CMAKE_CXX_STANDARD 17)

# 如果支持，请为 MSVC 编译器启用热重载。
if (POLICY CMP0141)
  cmake_policy(SET CMP0141 NEW)
  set(CMAKE_MSVC_DEBUG_INFORMATION_FORMAT "$<IF:$<AND:$<C_COMPILER_ID:MSVC>,$<CXX_COMPILER_ID:MSVC>>,$<$<CONFIG:Debug,RelWithDebInfo>:EditAndContinue>,$<$<CONFIG:Debug,RelWithDebInfo>:ProgramDatabase>>")
endif()

project ("tinyhttpd")

add_definitions(-DHV_STATICLIB)

include_directories(include/hv)

if(CMAKE_HOST_SYSTEM_NAME MATCHES "Linux")
	message("current platform: Linux ")
	link_directories(lib)
elseif(CMAKE_HOST_SYSTEM_NAME MATCHES "Windows")
	if(CMAKE_CL_64)
		set(CURRENT_PLATFORM "x64")
		message(STATUS "Current Platform is Windows ${CURRENT_PLATFORM}")
		link_directories(Windows/libx64)
	else(CMAKE_CL_64)
		set(CURRENT_PLATFORM "x86")
		message(STATUS "Current Platform is Windows ${CURRENT_PLATFORM}")
		link_directories(Windows/libx86)
	endif(CMAKE_CL_64)
else()
	message("current platform: unkonw ")
endif()
# 将源代码添加到此项目的可执行文件。
add_executable (tinyhttpd "tinyhttpd.cpp" "tinyhttpd.h")
target_link_libraries(tinyhttpd hv_static)

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET tinyhttpd PROPERTY CXX_STANDARD 20)
endif()

# TODO: 如有需要，请添加测试并安装目标。
