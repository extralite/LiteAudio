cmake_minimum_required(VERSION 3.0)
project(miniaudio)

set(MINIAUDIO_SOURCE
    ${CMAKE_CURRENT_SOURCE_DIR}/src/extras/miniaudio_split
)

set(MINIAUDIO_DEFINES
    MA_NO_DECODING
    MA_NO_ENCODING
    MA_NO_RESOURCE_MANAGER
    MA_NO_GENERATION
    MA_ENABLE_ONLY_SPECIFIC_BACKENDS
)

if(MSVC)
    # Always use DirectSound on Windows for compatibility
    list(APPEND MINIAUDIO_DEFINES MA_ENABLE_DSOUND)
endif()

if(UNIX AND ANDROID)
    list(APPEND MINIAUDIO_DEFINES MA_ENABLE_OPENSL)
endif()

if(UNIX AND APPLE)
    list(APPEND MINIAUDIO_DEFINES MA_ENABLE_COREAUDIO)
endif()

if(UNIX AND EMSCRIPTEN)
    list(APPEND MINIAUDIO_DEFINES MA_ENABLE_WEBAUDIO)
endif()

if(UNIX AND NOT ANDROID AND NOT APPLE AND NOT EMSCRIPTEN)
    # Always use ALSA on Linux systems for compatibility
    list(APPEND MINIAUDIO_DEFINES MA_ENABLE_ALSA)
endif()

add_library(miniaudio INTERFACE ${MINIAUDIO_SOURCE}/miniaudio.h)
target_sources(miniaudio INTERFACE ${MINIAUDIO_SOURCE}/miniaudio.c)
target_compile_definitions(miniaudio INTERFACE ${MINIAUDIO_DEFINES})
target_include_directories(miniaudio INTERFACE ${MINIAUDIO_SOURCE})
