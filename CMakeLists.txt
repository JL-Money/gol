# CMakeList.txt : Top-level CMake project file, do global configuration
# and include sub-projects here.
#
cmake_minimum_required (VERSION 3.8)

project ("gol" VERSION 0.0.2.1)

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
print_var(CMAKE_PROJECT_VERSION) #PROJECT_VERSION_MAJOR.MINOR.PATCH.TWEAK


# TODO: Add tests and install targets if needed.

#print_var(WIN32 UNIX MSVC MSVC_VERSION MSVC_TOOLSET_VERSION MINGW WIN WIN64 CMAKE_SYSTEM CMAKE_SYSTEM_NAME CMAKE_SYSTEM_VERSION CMAKE_SYSTEM_PROCESSOR CMAKE_HOST_SYSTEM_NAME CMAKE_HOST_APPLE
#  CMAKE_HOST_SYSTEM CMAKE_HOST_SYSTEM_NAME CMAKE_HOST_SYSTEM_PROCESSOR CMAKE_HOST_SYSTEM_VERSION CMAKE_HOST_WIN32 X64 X86 ARM64 ARM
#  CMAKE_EXE_LINKER_FLAGS
#)
function(dump_cmake_variables)
    get_cmake_property(_variableNames VARIABLES)
    list (SORT _variableNames)
    foreach (_variableName ${_variableNames})
        if (ARGV0)
            unset(MATCHED)
            string(REGEX MATCH ${ARGV0} MATCHED ${_variableName})
            if (NOT MATCHED)
                continue()
            endif()
        endif()
        message(STATUS "${_variableName}=${${_variableName}}")
    endforeach()
endfunction()
#dump_cmake_variables()
#execute_process(COMMAND "${CMAKE_COMMAND}" "-E" "environment")
