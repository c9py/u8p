cmake_minimum_required(VERSION 3.16)
project(Pentagram VERSION 1.0.0 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)

# Options
option(BUILD_LAUNCHER "Build unified launcher (requires SDL3)" ON)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

# Legacy autotools support note
message(STATUS "Note: This project supports both CMake and autotools build systems.")
message(STATUS "For traditional builds, use ./bootstrap && ./configure && make")

# Find dependencies
find_package(PkgConfig QUIET)

# Try to find SDL3 for launcher
if(BUILD_LAUNCHER)
    if(PKG_CONFIG_FOUND)
        pkg_check_modules(SDL3 QUIET sdl3)
        if(SDL3_FOUND)
            message(STATUS "SDL3 found: ${SDL3_VERSION}")
        else()
            message(WARNING "SDL3 not found. Launcher will not be built. Install SDL3 to enable launcher.")
            set(BUILD_LAUNCHER OFF)
        endif()
    else()
        message(WARNING "pkg-config not found. Cannot detect SDL3. Launcher will not be built.")
        set(BUILD_LAUNCHER OFF)
    endif()
endif()

# Shared components library
add_library(pentagram_shared STATIC
    # Audio components
    shared/audio/AudioChannel.cc
    shared/audio/AudioMixer.cc
    shared/audio/AudioSample.cc
    shared/audio/RawAudioSample.cc
    
    # File format utilities
    shared/files/Flex.cc
    shared/files/U7file.cc
    shared/files/utils.cc
    
    # Graphics scalers
    shared/scalers/BilinearScaler.cpp
    shared/scalers/BilinearScalerInternal_2x.cpp
    shared/scalers/BilinearScalerInternal_Arb.cpp
    shared/scalers/BilinearScalerInternal_X1Y12.cpp
    shared/scalers/BilinearScalerInternal_X2Y24.cpp
    shared/scalers/PointScaler.cpp
    shared/scalers/scale_2x.cc
    shared/scalers/scale_2xSaI.cc
    shared/scalers/scale_bilinear.cc
    shared/scalers/scale_hq2x.cc
    shared/scalers/scale_hq3x.cc
    shared/scalers/scale_hq4x.cc
    shared/scalers/scale_interlace.cc
    shared/scalers/scale_point.cc
    shared/scalers/scale_xbr.cc
)

target_include_directories(pentagram_shared PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${CMAKE_CURRENT_SOURCE_DIR}/shared
    ${CMAKE_CURRENT_SOURCE_DIR}/shared/audio
    ${CMAKE_CURRENT_SOURCE_DIR}/shared/files
    ${CMAKE_CURRENT_SOURCE_DIR}/shared/scalers
)

# Unified launcher (optional, requires SDL3)
if(BUILD_LAUNCHER)
    add_subdirectory(launcher)
endif()

# Install rules
install(TARGETS pentagram_shared
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
)

install(DIRECTORY shared/
    DESTINATION include/pentagram
    FILES_MATCHING PATTERN "*.h"
)

# Summary
message(STATUS "")
message(STATUS "Pentagram Build Configuration:")
message(STATUS "  Build system:        CMake + autotools")
message(STATUS "  Unified launcher:    ${BUILD_LAUNCHER}")
if(BUILD_LAUNCHER)
    message(STATUS "  SDL3 Version:        ${SDL3_VERSION}")
endif()
message(STATUS "  Shared library:      pentagram_shared")
message(STATUS "")
message(STATUS "Available targets:")
message(STATUS "  - pentagram_shared: Core shared components library")
if(BUILD_LAUNCHER)
    message(STATUS "  - pentagram-launcher: Unified game launcher")
endif()
message(STATUS "")
