cmake_minimum_required(VERSION 3.5)
project(SDL2_Libs_CI)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
set(CMAKE_C_FGLAGS "${CMAKE_C_FGLAGS}")

SET(CMAKE_FIND_LIBRARY_SUFFIXES .a ${CMAKE_FIND_LIBRARY_SUFFIXES})
add_compile_options("-Wno-narrowing")
set(CMAKE_CXX_STANDARD 14)

set(BUILD_SHARED_LIBS FALSE)
set(SKIP_INSTALL_LIBRARIES ON)
set(SDL_SHARED OFF)
set(SDL_VIDEO_DRIVER_X11_SUPPORTS_GENERIC_EVENTS ON)

set(SDL2TTF_INSTALL OFF)
set(SDL2TTF_HARFBUZZ OFF)
set(SDL2TTF_FREETYPE OFF)

set(FT_DISABLE_BROTLI ON)
set(FT_DISABLE_ZLIB ON)
set(FT_DISABLE_BZIP2 ON)
set(FT_DISABLE_PNG ON)
set(FT_DISABLE_HARFBUZZ ON)
 
set(SDL2IMAGE_INSTALL OFF)
set(SDL2IMAGE_DEPS_SHARED OFF)
set(SDL2IMAGE_JPEG OFF)
set(SDL2IMAGE_WEBP OFF)
set(SDL2IMAGE_PNG_SHARED OFF)

set(SDL2MIXER_FLAC OFF)
set(SDL2MIXER_OPUS OFF)
set(SDL2MIXER_MIDI OFF)
set(SDL2MIXER_INSTALL OFF)
set(SDL2MIXER_WAVPACK OFF)
set(SDL2MIXER_MOD OFF)

add_subdirectory(SDL2 ./SDL2_deps_build/ EXCLUDE_FROM_ALL)
add_subdirectory(SDL2_mixer ./SDL2_mixer_deps_build/ EXCLUDE_FROM_ALL)
add_subdirectory(SDL2_ttf ./SDL2_ttf_deps_build/ EXCLUDE_FROM_ALL)
add_subdirectory(SDL2_image ./SDL2_image_deps_build/ EXCLUDE_FROM_ALL)
add_subdirectory(SDL2_gfx ./SDL2_gfx_deps_build/ EXCLUDE_FROM_ALL)
add_subdirectory(freetype ./freetype_deps_build/ EXCLUDE_FROM_ALL)

add_executable(SDL2_Libs_CI build.cpp)

target_link_libraries(SDL2_Libs_CI PRIVATE SDL2 SDL2_ttf SDL2_mixer SDL2_image SDL2_gfx freetype)

add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/SDL2_deps_build/libSDL2.a ${CMAKE_SOURCE_DIR})
add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/SDL2_mixer_deps_build/libSDL2_mixer.a ${CMAKE_SOURCE_DIR})
add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/SDL2_ttf_deps_build/libSDL2_ttf.a ${CMAKE_SOURCE_DIR})
add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/SDL2_image_deps_build/libSDL2_image.a ${CMAKE_SOURCE_DIR})
add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/SDL2_gfx_deps_build/libSDL2_gfx.a ${CMAKE_SOURCE_DIR})
add_custom_command(TARGET SDL2_Libs_CI POST_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${CMAKE_BINARY_DIR}/freetype_deps_build/libfreetype.a ${CMAKE_SOURCE_DIR})