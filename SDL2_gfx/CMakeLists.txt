cmake_minimum_required(VERSION 3.10)

if(CMAKE_VERSION VERSION_GREATER_EQUAL 3.24)
  cmake_policy(SET CMP0135 NEW)
endif()

project(SDL2_gfx C)

set(SDL2_gfx_sources
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL2_framerate.c
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL2_gfxPrimitives.c
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL2_imageFilter.c
  ${CMAKE_CURRENT_SOURCE_DIR}/SDL2_rotozoom.c)
 
if(SDL2GFX_BUILD_SHARED_LIBS)
    set(sdl2_gfx_export_name SDL2_gfx)
    set(sdl2_gfx_install_name_infix shared)
    set(sdl2_target_name SDL2::SDL2)
else()
    set(sdl2_gfx_export_name SDL2_gfx-static)
    set(sdl2_gfx_install_name_infix static)
    set(sdl2_target_name SDL2::SDL2-static)
endif()

set(SDL_REQUIRED_VERSION 2.0.9)

sdl_find_sdl2(${sdl2_target_name} ${SDL_REQUIRED_VERSION})

set(BUILD_SHARED_LIBS ${SDL2GFX_BUILD_SHARED_LIBS})

add_library(
  ${PROJECT_NAME}
  STATIC
  ${SDL2_gfx_sources})

target_compile_definitions(
  ${PROJECT_NAME}
  PUBLIC
  ${GCC_MODULE_DEFS})

target_include_directories(
  ${PROJECT_NAME}
  PUBLIC
  ${CMAKE_CURRENT_BINARY_DIR}/SDL2/include)
  
target_link_libraries(${PROJECT_NAME} PRIVATE $<BUILD_INTERFACE:${sdl2_target_name}>)

target_include_directories(${PROJECT_NAME}
    PUBLIC
        "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>"
        "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}/SDL2>"
)

if (CMAKE_C_COMPILER_ID STREQUAL "Clang")
  set(COMPILE_OPTIONS
    -Wall
    -Wextra
    -Wpedantic
    -Wcast-align
    -Wpointer-arith
    -Wshadow
    -Wsign-compare
    -Wundef
    -Wwrite-strings)

elseif (CMAKE_C_COMPILER_ID STREQUAL "GNU")
  set(COMPILE_OPTIONS
    -Wall
    -Wextra
    -Wpedantic
    -Wcast-align
    -Wpointer-arith
    -Wshadow
    -Wsign-compare
    -Wundef
    -Wwrite-strings)

elseif (CMAKE_C_COMPILER_ID STREQUAL "MSVC")
  set(COMPILE_OPTIONS
    /W4
    /utf-8)
endif()