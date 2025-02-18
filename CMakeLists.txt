cmake_minimum_required(VERSION 3.10)

# Make sure this matches ./NAM/version.h!
project(NAM VERSION 0.2.0)

set(CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED OFF)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -stdlib=libc++ -std=c++20")

if (CMAKE_SYSTEM_NAME STREQUAL "Darwin")
    include_directories(SYSTEM /usr/local/include)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Linux")
    link_libraries(stdc++fs)
elseif (CMAKE_SYSTEM_NAME STREQUAL "Windows")
    add_compile_definitions(NOMINMAX WIN32_LEAN_AND_MEAN)
else()
    message(FATAL_ERROR "Unrecognized Platform!")
endif()

set(NAM_DEPS_PATH "${CMAKE_CURRENT_SOURCE_DIR}/Dependencies")

# Add a shared library target for libNAM.so
add_library(NAM SHARED
    NAM/activations.cpp
    NAM/convnet.cpp
    NAM/dsp.cpp
    NAM/get_dsp.cpp
    NAM/lstm.cpp
    NAM/util.cpp
)

target_include_directories(NAM PRIVATE ${CMAKE_SOURCE_DIR}/NAM)
target_link_libraries(NAM PRIVATE stdc++fs)

set_target_properties(NAM PROPERTIES OUTPUT_NAME "NAM")

# Ensure the shared library is built
install(TARGETS NAM LIBRARY DESTINATION /usr/local/lib)

# Build tools as usual
add_subdirectory(tools)
