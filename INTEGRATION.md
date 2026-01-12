# Ultimain Features Integration

This document describes the features integrated from the [ultimain](https://github.com/c9py/ultimain) repository into Pentagram.

## Overview

The ultimain repository contains advanced features for Ultima engines including a unified launcher, shared components library, cognitive NPC architecture, and web-based launcher. This integration brings these features into the Pentagram codebase.

## Integrated Components

### 1. Shared Components Library

Located in `shared/`, this library provides common functionality for Ultima engines:

#### Audio Subsystem (`shared/audio/`)
- **AudioChannel**: Multi-channel audio management
- **AudioMixer**: Audio mixing and playback
- **AudioSample**: Audio sample handling
- **RawAudioSample**: Raw PCM audio support

#### File Format Utilities (`shared/files/`)
- **Flex**: Ultima Flex file format support
- **U7file**: Ultima VII file handling
- **utils**: General file utilities and helpers
- **databuf**: Data buffer management

#### Graphics Scalers (`shared/scalers/`)
High-quality image scaling algorithms:
- Point scaling (nearest neighbor)
- Bilinear scaling
- Scale2x
- 2xSaI, Super2xSaI, SuperEagle
- HQ2x, HQ3x, HQ4x (high-quality scalers)
- XBR scaler
- Interlace scaler

### 2. Unified Launcher

Located in `launcher/`, an SDL3-based launcher that provides a unified interface for multiple Ultima engines:

**Features:**
- Visual game selection interface
- Automatic game detection
- Support for multiple engines:
  - Pentagram (Ultima VIII: Pagan)
  - Exult (Ultima VII: The Black Gate, Serpent Isle)
  - ScummVM (Ultima VIII alternative)
- Game availability indicators
- Keyboard navigation

**Building the Launcher:**
```bash
# Install SDL3 first
git clone --depth 1 https://github.com/libsdl-org/SDL.git SDL3
cd SDL3 && mkdir build && cd build
cmake .. && make -j$(nproc) && sudo make install && sudo ldconfig

# Build Pentagram with launcher
mkdir build && cd build
cmake .. -DBUILD_LAUNCHER=ON
make pentagram-launcher
```

**Running the Launcher:**
```bash
./launcher/pentagram-launcher
```

### 3. Web Launcher

Located in `web/`, a browser-based launcher interface:

**Features:**
- Modern, responsive web interface
- Visual game cards with descriptions
- Status indicators for game availability
- Styled with CSS gradients and animations
- Ready for CheerpX integration (future)

**Usage:**
Simply open `web/index.html` in a modern web browser.

### 4. Cognitive NPC Architecture Documentation

Located in `docs/`, comprehensive documentation for advanced AI/NPC systems:

- **cognitive_npc_architecture.md**: Neuro-symbolic cognitive NPC system design
  - Tensor Logic for reasoning
  - 4th-Order AIML for meta-cognitive dialogue
  - Tiny Native LLM for novelty generation
  - Sims-style social dynamics
  - Dream Vortex persona models
  
- **npc_ai_implementation_report.md**: Implementation details and roadmap

- **exult_npc_analysis.md**: Analysis of Exult's NPC system

- **world_map_structure.md**: World map and spatial data structures

- **gis_format_comparison.md**: GIS format comparison for world data

## Build Systems

This project now supports both build systems:

### CMake (New)
Modern, cross-platform build system for shared components and launcher:

```bash
mkdir build && cd build
cmake .. -DBUILD_LAUNCHER=ON
make
sudo make install
```

### Autotools (Legacy)
Traditional build system for main Pentagram engine:

```bash
./bootstrap
./configure
make
sudo make install
```

## Dependencies

### Core Dependencies (Autotools Build)
- SDL 1.2 or 2.0
- SDL_ttf
- zlib
- libpng

### Additional Dependencies (CMake + Launcher)
- SDL3 3.5.0+
- CMake 3.16+
- C++17 compatible compiler

## Project Structure

```
u8p/
├── CMakeLists.txt          # Root CMake configuration
├── INTEGRATION.md          # This file
├── shared/                 # Shared components library
│   ├── audio/             # Audio subsystem
│   ├── files/             # File format utilities
│   ├── scalers/           # Graphics scalers
│   └── *.h                # Common headers
├── launcher/              # Unified game launcher
│   ├── CMakeLists.txt
│   └── main.cpp
├── web/                   # Web-based launcher
│   └── index.html
├── docs/                  # Enhanced documentation
│   ├── cognitive_npc_architecture.md
│   ├── npc_ai_implementation_report.md
│   ├── exult_npc_analysis.md
│   ├── world_map_structure.md
│   └── gis_format_comparison.md
└── [original pentagram files...]
```

## Future Work

### Planned Enhancements
1. **Complete Engine Integration**: Fully integrate Pentagram engine with shared library
2. **CheerpX Support**: Enable running engines in browser via WebAssembly
3. **Cognitive NPC Implementation**: Implement advanced AI features from documentation
4. **Data File Management**: Create unified system for managing game data files
5. **Cross-Engine Asset Sharing**: Enable sharing of assets between engines

### Experimental Features
The cognitive architecture documentation describes several experimental AI/ML features:
- Neural network integration via GNeural-Net
- AGML-based dialogue system with meta-cognition
- LLM integration for dynamic content generation
- Social dynamics simulation
- Economic simulation systems

These are documented for research and future development.

## License

This integration maintains GPL-2.0 licensing consistent with both:
- Pentagram (GPL-2.0)
- Ultimain components (GPL-2.0)

See COPYING for full license text.

## Credits

- **Pentagram Team**: Original Pentagram engine
- **Ultimain Project**: Shared components, launcher, and cognitive architecture
- **Exult Team**: Ultima VII engine and file format documentation
- **ScummVM Team**: Ultima VIII engine support

## References

- Pentagram: https://github.com/c9py/u8p
- Ultimain: https://github.com/c9py/ultimain
- Exult: http://exult.sourceforge.net/
- ScummVM: https://www.scummvm.org/
