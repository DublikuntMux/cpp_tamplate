cmake_minimum_required(VERSION 3.25.0)

project(LIB_NAME
        VERSION 1.0.1
        DESCRIPTION ""
        LANGUAGES CXX
)

file(GLOB_RECURSE lib_sources CONFIGURE_DEPENDS "./src/*.cpp")
file(GLOB_RECURSE lib_header CONFIGURE_DEPENDS "./include/*.hpp")

add_library(LIB_NAME ${lib_sources} ${lib_header})
target_include_directories(LIB_NAME PUBLIC include/)

set_target_properties(LIB_NAME PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(LIB_NAME PROPERTIES SOVERSION 1)
