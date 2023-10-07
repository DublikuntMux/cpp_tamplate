cmake_minimum_required(VERSION 3.25.0)
project(MAIN_NAME
        VERSION 1.0.1
        LANGUAGES CXX
)

if(PROJECT_SOURCE_DIR STREQUAL PROJECT_BINARY_DIR)
  message(
    FATAL_ERROR
      "In-source builds not allowed. Please make a new directory (called a build directory) and run CMake from there."
  )
endif()

include(FetchContent)

include("cmake/Cache.cmake")
include("cmake/CompilerWarnings.cmake")
include("cmake/StaticAnalyzers.cmake")
include("cmake/InterproceduralOptimization.cmake") 

option(MAIN_NAME_ENABLE_IPO "Enable IPO/LTO" OFF)
option(MAIN_NAME_ENABLE_CACHE "Enable ccache" OFF)
option(MAIN_NAME_WARNINGS_AS_ERRORS "Treat Warnings As Errors" OFF)
option(MAIN_NAME_ENABLE_SANITIZER_ADDRESS "Enable address sanitizer" OFF)
option(MAIN_NAME_ENABLE_SANITIZER_LEAK "Enable leak sanitizer" OFF)
option(MAIN_NAME_ENABLE_SANITIZER_UNDEFINED "Enable undefined sanitizer" OFF)
option(MAIN_NAME_ENABLE_SANITIZER_THREAD "Enable thread sanitizer" OFF)
option(MAIN_NAME_ENABLE_SANITIZER_MEMORY "Enable memory sanitizer" OFF)

option(MAIN_NAME_ENABLE_TESTS "Enable tests" OFF)

if (NOT DEFINED CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()

set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_VISIBILITY_PRESET hidden)

if(MAIN_NAME_ENABLE_IPO)
    enable_ipo()
endif()

if(MAIN_NAME_ENABLE_CACHE)
    enable_cache()
endif()

add_subdirectory("lib")


if(MAIN_NAME_ENABLE_TESTS)
    add_subdirectory("tests")
endif()

set_project_warnings(
    LIB_NAME
    ${MAIN_NAME_WARNINGS_AS_ERRORS}
    ""
    ""
    ""
)

if(MAIN_NAME_ENABLE_TESTS)
    set_project_warnings(
        LIB_TESTS
        ${MAIN_NAME_WARNINGS_AS_ERRORS}
        ""
        ""
        ""
    )
endif()


enable_sanitizers(
    LIB_NAME
    ${MAIN_NAME_ENABLE_SANITIZER_ADDRESS}
    ${MAIN_NAME_ENABLE_SANITIZER_LEAK}
    ${MAIN_NAME_ENABLE_SANITIZER_UNDEFINED}
    ${MAIN_NAME_ENABLE_SANITIZER_THREAD}
    ${MAIN_NAME_ENABLE_SANITIZER_MEMORY}
)

if(MAIN_NAME_ENABLE_TESTS)
    enable_sanitizers(
        LIB_TESTS
        ${MAIN_NAME_ENABLE_SANITIZER_ADDRESS}
        ${MAIN_NAME_ENABLE_SANITIZER_LEAK}
        ${MAIN_NAME_ENABLE_SANITIZER_UNDEFINED}
        ${MAIN_NAME_ENABLE_SANITIZER_THREAD}
        ${MAIN_NAME_ENABLE_SANITIZER_MEMORY}
    )
endif()
