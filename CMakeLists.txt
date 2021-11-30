cmake_minimum_required(VERSION 3.5)
project(opengl_test_conan CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Download automatically, you can also just copy the conan.cmake file
if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
   message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
   file(DOWNLOAD "https://raw.githubusercontent.com/conan-io/cmake-conan/master/conan.cmake"
                  "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_cmake_run(REQUIRES 
		  glfw/3.3.4
		  glm/0.9.9.5		
                BASIC_SETUP 
		CMAKE_TARGETS)


set(GLAD_SOURCES glad.c )
set_source_files_properties(${GLAD_SOURCES} PROPERTIES LANGUAGE CXX )
add_library(glad STATIC ${GLAD_SOURCES})

target_include_directories(glad PRIVATE include)

add_executable(opengl_test_conan main.cpp)

target_include_directories(opengl_test_conan 
	PRIVATE 
	 include  
)

target_link_libraries(opengl_test_conan  CONAN_PKG::glfw CONAN_PKG::glm glad)

enable_testing()

add_test(
  NAME opengl_test_conan
  COMMAND $<TARGET_FILE:opengl_test_conan>
  )


