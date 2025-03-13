cmake_minimum_required(VERSION 3.16)

project(deepstream_app)
find_package(nvds_service_maker REQUIRED PATHS /opt/nvidia/deepstream/deepstream/service-maker/cmake)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
add_executable(my-deepstream-app deepstream.cpp)
target_link_libraries(my-deepstream-app PRIVATE  nvds_service_maker)