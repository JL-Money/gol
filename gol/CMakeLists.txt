# CMakeList.txt : CMake project for gol, include source and define
# project specific logic here.
#

# Add source to this project's executable.
add_executable (gol "gol.cpp" "gol.h")

if (CMAKE_VERSION VERSION_GREATER 3.12)
  set_property(TARGET gol PROPERTY CXX_STANDARD 20)
endif()

# TODO: linking dependencies
target_link_libraries(gol PRIVATE vuda)

target_include_directories(gol PUBLIC ${glfw_path}/include)
if(CMAKE_HOST_WIN32)
  target_link_libraries(gol PUBLIC ${glfw_path}/lib-vc2022/glfw3.lib)
elseif(CMAKE_HOST_APPLE)
  target_link_libraries(gol PUBLIC ${glfw_path}/lib-vc2022/glfw3.lib)
else()
  target_link_libraries(gol PUBLIC ${glfw_path}/lib-vc2022/glfw3.lib)
endif()
