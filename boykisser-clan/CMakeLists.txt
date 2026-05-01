cmake_minimum_required(VERSION 3.20)
project(boykisser-clan)

# Set C++ standard
set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Find vcpkg packages
find_package(OpenSSL REQUIRED)
find_package(zstd REQUIRED)
find_package(xxhash REQUIRED)

# Collect source files
file(GLOB LUAU_SOURCES "src/Luau/*.cpp")
file(GLOB UTILS_SOURCES "include/utils/*.cpp")

set(SOURCES
    src/server.cpp
    src/worker.cpp
    src/boykisser-clan.cpp
    ${LUAU_SOURCES}
    ${UTILS_SOURCES}
)

# Create shared library
add_library(boykisser-clan SHARED ${SOURCES})

# Include directories
target_include_directories(boykisser-clan PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_SOURCE_DIR}/include/server
    ${CMAKE_CURRENT_SOURCE_DIR}/include/utils
    ${CMAKE_CURRENT_SOURCE_DIR}/include/Luau
)

# Link libraries
target_link_libraries(boykisser-clan PRIVATE
    OpenSSL::SSL
    OpenSSL::Crypto
    zstd::zstd
    xxhash::xxhash
)

# Windows-specific settings
if(WIN32)
    target_compile_definitions(boykisser-clan PRIVATE
        _WINDOWS
        _USRDLL
    )
endif()
