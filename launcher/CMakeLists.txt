# Pentagram Unified Launcher
# Requires SDL3 to be found by parent CMakeLists.txt

# Launcher executable
add_executable(pentagram-launcher
    main.cpp
)

target_include_directories(pentagram-launcher PRIVATE
    ${SDL3_INCLUDE_DIRS}
)

# Note: Using ${SDL3_LIBRARIES} instead of imported targets
# because SDL3 is found via pkg-config, which doesn't provide
# imported targets. When SDL3 provides CMake config files,
# prefer: target_link_libraries(pentagram-launcher PRIVATE SDL3::SDL3)
target_link_libraries(pentagram-launcher PRIVATE
    ${SDL3_LIBRARIES}
)

target_compile_options(pentagram-launcher PRIVATE
    ${SDL3_CFLAGS_OTHER}
)

# Install
install(TARGETS pentagram-launcher RUNTIME DESTINATION bin)

message(STATUS "Pentagram Launcher configured with SDL3 ${SDL3_VERSION}")
