cmake_minimum_required(VERSION 3.5)
project(opengl_test_conan CXX)

set(CMAKE_MODULE_PATH ${CMAKE_BINARY_DIR} ${CMAKE_MODULE_PATH})

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(OpenGL REQUIRED)
find_package(glfw3 REQUIRED)
find_package(glm REQUIRED)

set(GLAD_SOURCES glad.c )
set_source_files_properties(${GLAD_SOURCES} PROPERTIES LANGUAGE CXX )
add_library(glad STATIC ${GLAD_SOURCES})

target_include_directories(glad PRIVATE include)

add_executable(opengl_test_conan main.cpp)

target_include_directories(opengl_test_conan 
	PRIVATE 
	 include  
	 ${glm_INCLUDE_DIRS} 
)

target_link_libraries(opengl_test_conan  glfw::glfw glad)



