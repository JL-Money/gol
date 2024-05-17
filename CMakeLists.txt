# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("gol" VERSION 0.0.1)

function(print_var)
	foreach(var ${ARGN})
		message("${var} = ${${var}}")
	endforeach()
endfunction()

# Include sub-projects.
add_subdirectory ("gol")

# Include directories for vuda.hpp
add_library(vuda INTERFACE)
target_include_directories(vuda INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/ext/vuda/inc)

# Include directories for Vulkan
find_package(Vulkan REQUIRED)
#include_directories(${Vulkan_INCLUDE_DIRS}) # Unnecessary?
#target_link_libraries(vuda INTERFACE ${Vulkan_LIBRARIES}) # Unnecessary?
target_include_directories(vuda INTERFACE ${Vulkan_INCLUDE_DIRS})

set(vulkan_sdk_path "$ENV{VULKAN_SDK}")
print_var(vulkan_sdk_path  Vulkan_INCLUDE_DIRS  Vulkan_LIBRARIES)
print_var(CMAKE_CURRENT_SOURCE_DIR)
print_var(PROJECT_VERSION_MAJOR PROJECT_VERSION_MINOR PROJECT_VERSION_PATCH)
print_var(CMAKE_MODULE_PATH)

# TODO: Add tests and install targets if needed.
