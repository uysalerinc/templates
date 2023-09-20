cmake_minimum_required(VERSION 3.0)
project(Foo)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/modules/")
set(RES_DIR ${CMAKE_SOURCE_DIR}/res)


# Add libraries
link_directories(${CMAKE_SOURCE_DIR}/lib)

# Find Requirements
set(OpenGL_GL_PREFERENCE GLVND)
find_package(OpenGL REQUIRED)
add_definitions(${OPENGL_DEFINITIONS})

find_package(GLM REQUIRED)
message(STATUS "GLM included at ${GLM_INCLUDE_DIR}")

find_package(GLFW3 REQUIRED)
message(STATUS "Found GLFW3 in ${GLFW3_INCLUDE_DIR}")

find_package(ASSIMP REQUIRED)
message(STATUS "Found ASSIMP in ${ASSIMP_INCLUDE_DIR}")

# TODO
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -Wall")
find_package(X11 REQUIRED)


set(LIBS ${GLFW3_LIBRARY} X11 Xrandr Xinerama Xi Xxf86vm Xcursor GL dl pthread freetype ${ASSIMP_LIBRARY})
set (CMAKE_CXX_LINK_EXECUTABLE "${CMAKE_CXX_LINK_EXECUTABLE} -ldl")

# Configure file
configure_file(cmake/config/dir_helper.h.in cmake/config/dir_helper.h)
include_directories(${CMAKE_BINARY_DIR}/cmake/config)

# Static Libs
add_library(STB_IMAGE "thirdparty/stb_image.cpp")
set(LIBS ${LIBS} STB_IMAGE)

add_library(GLAD "thirdparty/glad.c")
set(LIBS ${LIBS} GLAD)

file(GLOB SOURCE
    ${CMAKE_SOURCE_DIR}/src/*.cpp
)

file(GLOB HEADER
    ${CMAKE_SOURCE_DIR}/src/*.h
)

add_executable(${PROJECT_NAME} ${SOURCE} ${HEADER})
target_link_libraries(${PROJECT_NAME} ${LIBS})
