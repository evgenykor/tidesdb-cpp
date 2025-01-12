cmake_minimum_required(VERSION 3.25)
project(tidesdb_cpp CXX)

set(CMAKE_CXX_STANDARD 20)
set(PROJECT_VERSION 0.1.0)
add_compile_options(-Wextra -Wall -Werror)

find_library(LIBRARY_TIDEDB NAMES tidesdb REQUIRED) # require tidesdb library

add_library(tidesdb_cpp SHARED tidesdb.cpp)
target_link_libraries(tidesdb_cpp PUBLIC ${LIBRARY_TIDEDB})


include(FetchContent)
FetchContent_Declare(
  googletest
  URL https://github.com/google/googletest/archive/03597a01ee50ed33e9dfd640b249b4be3799d395.zip
)

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
FetchContent_MakeAvailable(googletest)

enable_testing()

add_executable(tests test/tests.cpp)
target_link_libraries(tests GTest::gtest_main tidesdb_cpp ${LIBRARY_TIDEDB})

include(GoogleTest)
gtest_discover_tests(tests)
