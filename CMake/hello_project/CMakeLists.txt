# CMake version
cmake_minimum_required(VERSION 3.10)

include(${CMAKE_SOURCE_DIR}/GCC.cmake)
set(CMAKE_TOOLCHAIN_FILE ${CMAKE_SOURCE_DIR}/GCC.cmake)

target_include_directories(HelloWorld PRIVATE include)

# Khai báo dự án
project(HelloWorld C)

## 
add_executable(hello main.c)

set(BOARD stm32 CACHE STRING "Target board")



message("Hello")
message(STATUS "Hello STATUS")
message(DEBUG "Hello DEBUG")
message(WARNING "Hello WARNING")
message(FATAL ERROR "Hello FATAL ERROR")


