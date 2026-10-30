cmake_minimum_required(VERSION 3.22)

set(CMAKE_TOOLCHAIN_FILE ${CMAKE_CURRENT_SOURCE_DIR}/cmake/toolchain_config.cmake)

project(mik32-rps VERSION 1.0.0)
enable_language(C CXX ASM)

add_subdirectory(third_party)
add_subdirectory(firmware)
