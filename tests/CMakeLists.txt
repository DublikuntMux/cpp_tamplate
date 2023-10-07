cmake_minimum_required(VERSION 3.25.0)

project(LIB_TESTS
        VERSION 1.0.1
        DESCRIPTION "LIB_NAME tests"
        LANGUAGES CXX
)

file(GLOB_RECURSE tests_sources CONFIGURE_DEPENDS "./src/*.cpp")

add_executable(LIB_TESTS ${tests_sources})

set_target_properties(LIB_TESTS PROPERTIES VERSION ${PROJECT_VERSION})
set_target_properties(LIB_TESTS PROPERTIES SOVERSION 1)

FetchContent_Declare(
        googletest
        GIT_REPOSITORY https://github.com/google/googletest.git
        GIT_TAG main
)

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)
enable_testing()

target_link_libraries(LIB_TESTS PRIVATE 
        GTest::gtest_main
        LIB_NAME
)
target_include_directories(LIB_TESTS PRIVATE 
        LIB_NAME
)

include(GoogleTest)
gtest_discover_tests(LIB_TESTS)
