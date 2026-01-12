# Ultimain Integration Quick Start

This directory contains features integrated from the [ultimain](https://github.com/c9py/ultimain) repository.

## What's New

### 1. Unified Game Launcher (`launcher/`)
SDL3-based visual launcher for Ultima games including Pentagram, Exult, and ScummVM engines.

**Try it:**
```bash
# Install SDL3, then:
mkdir build && cd build
cmake .. -DBUILD_LAUNCHER=ON
make pentagram-launcher
./pentagram-launcher
```

### 2. Web Launcher (`web/`)
Browser-based game launcher with modern UI.

**Try it:**
```bash
# Open in your browser:
firefox web/index.html
```

### 3. Shared Components Library (`shared/`)
Audio, file format, and graphics scaler utilities from ultimain.

Components include:
- Audio subsystem (mixing, channels, samples)
- File format utilities (Flex, U7file)
- High-quality graphics scalers (HQ2x/3x/4x, 2xSaI, XBR, etc.)

**Status:** Experimental - requires full Pentagram dependencies to build.

### 4. Advanced AI Documentation (`docs/`)
Documentation for cognitive NPC architecture including:
- Neuro-symbolic reasoning systems
- Meta-cognitive dialogue (4th-order AIML)
- Social dynamics simulation
- World map structures

## Documentation

See [INTEGRATION.md](INTEGRATION.md) for complete documentation including:
- Detailed feature descriptions
- Build instructions
- Dependencies
- Project structure
- Future roadmap

## Quick Reference

### Build Main Pentagram (Recommended)
```bash
./bootstrap
./configure
make
```

### Build Launcher Only
```bash
# Install SDL3 first
mkdir build && cd build
cmake .. -DBUILD_LAUNCHER=ON
make pentagram-launcher
```

### CMake Options
- `BUILD_LAUNCHER=ON/OFF` - Build SDL3 launcher (requires SDL3)
- `BUILD_SHARED_COMPONENT=ON/OFF` - Build shared library (experimental)

## Requirements

### Core Pentagram
- SDL 1.2 or 2.0
- SDL_ttf
- zlib
- libpng

### Launcher (Optional)
- SDL3 3.5.0+
- CMake 3.16+
- C++17 compiler

## License

GPL-2.0 (consistent with Pentagram and ultimain)

## Credits

- Pentagram Team - Original Pentagram engine
- Ultimain Project - Integrated features and documentation
- Exult Team - File format documentation
- ScummVM Team - Ultima engine support
