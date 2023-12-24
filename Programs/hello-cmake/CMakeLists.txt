cmake_minimum_required(VERSION 3.20)

project(hello-cmake)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_executable(${PROJECT_NAME}
    hello-cmake.cpp
)

set_target_properties(${PROJECT_NAME} PROPERTIES OUTPUT_NAME "out")