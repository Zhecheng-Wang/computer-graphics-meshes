cmake_minimum_required(VERSION 3.10)
project(meshesh)

if (NOT CMAKE_BUILD_TYPE)
	set(CMAKE_BUILD_TYPE "Release")
endif()

list(PREPEND CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

### libIGL options: choose between header only and compiled static library
# include libigl png
include(libigl)
igl_include(glfw)
igl_include(opengl)
igl_include(stb)

# Add your project files
include_directories("${CMAKE_CURRENT_SOURCE_DIR}/include/")
if(USE_SOLUTION)
  file(GLOB SRCFILES solution/*.cpp)
else()
  file(GLOB SRCFILES src/*.cpp)
endif()

add_library(core ${SRCFILES})
target_link_libraries(core igl::core igl::opengl igl::glfw )

add_executable(normals "normals.cpp")
target_link_libraries(normals core igl::core igl::opengl igl::glfw )

add_executable(quad_subdivision "quad_subdivision.cpp")
target_link_libraries(quad_subdivision core igl::core igl::opengl igl::glfw )

add_executable(obj "obj.cpp")
target_link_libraries(obj core igl::stb igl::core igl::opengl igl::glfw )
