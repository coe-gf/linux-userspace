cmake_minimum_required(VERSION 3.14 FATAL_ERROR)

# ---- Project ----

project(standalone LANGUAGES C)

# ---- Create executable ----
set(CMAKE_BUILD_TYPE "Debug")
set(CMAKE_C_FLAGS_DEBUG "$ENV{CMAKE_CXX_FLAGS} -O0 -Wall -g")

# add your source files here

file(GLOB_RECURSE headers CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/include/*.h")
file(GLOB_RECURSE headers CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.h")
file(GLOB_RECURSE sources CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c")

add_library(standalone_lib ${headers} ${sources})
target_include_directories(
  standalone_lib PUBLIC $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
                        $<INSTALL_INTERFACE:include/standalone_lib-${PROJECT_VERSION}>
)

file(GLOB sources CONFIGURE_DEPENDS ${CMAKE_CURRENT_SOURCE_DIR}/test/src/*.c)

add_executable(standalone_bin ${sources})
# link to your dependencies' targets here
target_link_libraries(standalone_bin standalone_lib)
# setup your target's properties
set_target_properties(standalone_bin PROPERTIES C_STANDARD 11)
