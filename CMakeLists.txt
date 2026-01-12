cmake_minimum_required(VERSION 3.16)
project(Pentagram VERSION 1.0.0 LANGUAGES CXX C)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_C_STANDARD 11)

# Options
option(BUILD_LAUNCHER "Build unified launcher (requires SDL3)" ON)
option(BUILD_SHARED_COMPONENT "Build shared components library (experimental)" OFF)

# Legacy autotools support note
message(STATUS "")
message(STATUS "==========================================================")
message(STATUS "Pentagram CMake Build System (Experimental)")
message(STATUS "==========================================================")
message(STATUS "")
message(STATUS "This CMake build system provides support for:")
message(STATUS "  - Unified game launcher (SDL3-based)")
message(STATUS "  - Shared components library (audio, files, scalers)")
message(STATUS "")
message(STATUS "For building the main Pentagram engine, use:")
message(STATUS "  ./bootstrap && ./configure && make")
message(STATUS "")
message(STATUS "==========================================================")
message(STATUS "")

# Find dependencies
find_package(PkgConfig QUIET)

# Try to find SDL3 for launcher
if(BUILD_LAUNCHER)
    if(PKG_CONFIG_FOUND)
        pkg_check_modules(SDL3 QUIET sdl3)
        if(SDL3_FOUND)
            message(STATUS "SDL3 found: ${SDL3_VERSION}")
        else()
            message(WARNING "SDL3 not found. Launcher will not be built.")
            message(STATUS "To install SDL3:")
            message(STATUS "  git clone --depth 1 https://github.com/libsdl-org/SDL.git SDL3")
            message(STATUS "  cd SDL3 && mkdir build && cd build")
            message(STATUS "  cmake .. && make -j\$(nproc) && sudo make install && sudo ldconfig")
            set(BUILD_LAUNCHER OFF)
        endif()
    else()
        message(WARNING "pkg-config not found. Cannot detect SDL3. Launcher will not be built.")
        set(BUILD_LAUNCHER OFF)
    endif()
endif()

# Shared components library (experimental - requires Pentagram dependencies)
if(BUILD_SHARED_COMPONENT)
    message(STATUS "Building shared components library (experimental)")
    
    # Note: This library has dependencies on the main Pentagram codebase
    # For now, this is a placeholder for future integration
    
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
        ${CMAKE_CURRENT_SOURCE_DIR}/misc
        ${CMAKE_CURRENT_SOURCE_DIR}/filesys
        ${CMAKE_CURRENT_SOURCE_DIR}/graphics
    )
    
    # Install rules
    install(TARGETS pentagram_shared
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib
    )

    install(DIRECTORY shared/
        DESTINATION include/pentagram
        FILES_MATCHING PATTERN "*.h"
    )
endif()

# Unified launcher (optional, requires SDL3)
if(BUILD_LAUNCHER)
    add_subdirectory(launcher)
endif()

# Summary
message(STATUS "")
message(STATUS "Pentagram Build Configuration:")
message(STATUS "  Build system:        CMake (experimental) + autotools (primary)")
message(STATUS "  Unified launcher:    ${BUILD_LAUNCHER}")
if(BUILD_LAUNCHER)
    message(STATUS "  SDL3 Version:        ${SDL3_VERSION}")
endif()
message(STATUS "  Shared library:      ${BUILD_SHARED_COMPONENT}")
if(BUILD_SHARED_COMPONENT)
    message(STATUS "    Note: Shared library requires full Pentagram dependencies")
endif()
message(STATUS "")
message(STATUS "Available targets:")
if(BUILD_SHARED_COMPONENT)
    message(STATUS "  - pentagram_shared: Core shared components library")
endif()
if(BUILD_LAUNCHER)
    message(STATUS "  - pentagram-launcher: Unified game launcher")
endif()
if(NOT BUILD_LAUNCHER AND NOT BUILD_SHARED_COMPONENT)
    message(STATUS "  (No optional components configured)")
    message(STATUS "  Enable with: cmake -DBUILD_LAUNCHER=ON -DBUILD_SHARED_COMPONENT=ON")
endif()
message(STATUS "")
