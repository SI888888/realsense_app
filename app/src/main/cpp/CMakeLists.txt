# Sets the minimum version of CMake required to build the native library.
cmake_minimum_required(VERSION 3.5)
project(librealsense)

# Set Android-specific variables
set(ANDROID on CACHE BOOL "ANDROID")
set(APP_PLATFORM "android-21" CACHE STRING "ANDROID")
set(BUILD_UNIT_TESTS off CACHE BOOL "BUILD_UNIT_TESTS")
set(BUILD_EXAMPLES off CACHE BOOL "BUILD_EXAMPLES")
set(FORCE_LIBUVC on CACHE BOOL "FORCE_LIBUVC")

# Add the native-lib shared library
add_library(
        native-lib
        SHARED
        native-lib.cpp
)

# Find the log library from NDK
find_library(
        log-lib
        log
)

# Define the path to the precompiled librealsense2.so file
add_library(
        realsense2
        SHARED
        IMPORTED
)

# Specify the location of the realsense2.so file
set_target_properties(
        realsense2
        PROPERTIES
        IMPORTED_LOCATION "${CMAKE_SOURCE_DIR}/librealsense/build/librealsense2.so" # 确保这里是你生成的librealsense2.so的正确路径
)

# 设置native-lib库的输出路径
set_target_properties(
        native-lib
        PROPERTIES
        LIBRARY_OUTPUT_DIRECTORY "/Users/apple/AndroidStudioProjects/realsense_app/app/src/main/cpp/librealsense/build"
)

# Link the native-lib library with realsense2 and log libraries
target_link_libraries(
        native-lib
        realsense2
        ${log-lib}
)


