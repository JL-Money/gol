# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("gol" VERSION 0.0.2)

function(print_var)
	foreach(var ${ARGN})
		message("${var} = ${${var}}")
	endforeach()
endfunction()

# Initialize path variables
set(glfw_path "C:/Users/josep/Documents/OpenGL/glfw-3.4.bin.WIN64/")
set(vulkan_sdk_path "$ENV{VULKAN_SDK}")
print_var(glfw_path vulkan_sdk_path)

# Include sub-projects.
add_subdirectory ("gol")

# Include directories for VUDA
add_library(vuda INTERFACE)
target_include_directories(vuda INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/ext/vuda/inc)

# Include directories for Vulkan
find_package(Vulkan REQUIRED)
if(Vulkan_FOUND)
  message(STATUS "Vulkan found: ${Vulkan_LIBRARY}")
endif()
target_include_directories(vuda INTERFACE ${Vulkan_INCLUDE_DIRS})
target_link_libraries(vuda INTERFACE ${Vulkan_LIBRARIES}) # Without this, vk functions are inaccessible

print_var(CMAKE_CURRENT_SOURCE_DIR)
print_var(Vulkan_INCLUDE_DIRS Vulkan_LIBRARIES)
print_var(PROJECT_VERSION_MAJOR PROJECT_VERSION_MINOR PROJECT_VERSION_PATCH)

# TODO: Add tests and install targets if needed.
