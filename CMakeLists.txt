cmake_minimum_required(VERSION 3.10)
project(rapidjson_vendor)

find_package(ament_cmake REQUIRED)
include(ExternalProject)

ExternalProject_Add(rapidjson_src
  GIT_REPOSITORY https://github.com/Tencent/rapidjson.git
  GIT_TAG v1.1.0
  CONFIGURE_COMMAND ""
  BUILD_COMMAND ""
  INSTALL_COMMAND ${CMAKE_COMMAND} -E copy_directory
    <SOURCE_DIR>/include ${CMAKE_CURRENT_BINARY_DIR}/include
  PATCH_COMMAND ${CMAKE_COMMAND} -E remove_directory <SOURCE_DIR>/bin
  UPDATE_DISCONNECTED TRUE
)

add_library(rapidjson_vendor INTERFACE)
add_dependencies(rapidjson_vendor rapidjson_src)
target_include_directories(rapidjson_vendor INTERFACE
  $<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/include>
  $<INSTALL_INTERFACE:include>
)

install(
  DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/include/
  DESTINATION include
)

ament_export_include_directories(include)
ament_package()
