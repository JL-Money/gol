# CMakeList.txt : CMake project VUDA and Vulkan
#

# Include directories for VUDA
add_library(vuda INTERFACE)
target_include_directories(vuda INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/vuda/inc)

# Include directories for Vulkan
find_package(Vulkan REQUIRED)
if(Vulkan_FOUND)
  message(STATUS "Vulkan found: ${Vulkan_LIBRARY}")
endif()
target_include_directories(vuda INTERFACE ${Vulkan_INCLUDE_DIRS})
target_link_libraries(vuda INTERFACE ${Vulkan_LIBRARIES}) # Without this, vk functions are inaccessible