cmake_minimum_required(VERSION 3.8)

if(NOT WIN32)
    SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")
endif()

project(RealsensePCLExamples)

find_package( PCL REQUIRED )

if (PCL_FOUND)
  include_directories(${PCL_INCLUDE_DIRS})
  add_definitions(${PCL_DEFINITIONS})
  link_directories(${PCL_LIBRARY_DIRS})
endif()

list(APPEND DEPENDENCIES ${PCL_LIBRARIES})

find_package(OpenGL)
if(NOT OPENGL_FOUND)
    message(FATAL_ERROR "\n\n OpenGL package is missing!\n\n")
endif()

list(APPEND DEPENDENCIES ${OPENGL_LIBRARIES})

if(WIN32)
    list(APPEND DEPENDENCIES glfw3)
else()
    find_package(glfw3 REQUIRED)
    list(APPEND DEPENDENCIES glfw)
endif()

add_subdirectory(pcl)
add_subdirectory(pcl-color)
