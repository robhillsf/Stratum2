# Stratum2 Project Structure

```
Stratum2/
├── CMakeLists.txt
├── README.md
├── .gitignore
├── Source/
│   ├── PluginProcessor.h
│   ├── PluginProcessor.cpp
│   ├── PluginEditor.h
│   ├── PluginEditor.cpp
│   ├── Core/
│   │   ├── ProjectManager.h
│   │   ├── ProjectManager.cpp
│   │   ├── MIDIProcessor.h
│   │   ├── MIDIProcessor.cpp
│   │   ├── TimingAnalyzer.h
│   │   ├── TimingAnalyzer.cpp
│   │   ├── LayerStackManager.h
│   │   ├── LayerStackManager.cpp
│   │   ├── HarmonicAnalyzer.h
│   │   ├── HarmonicAnalyzer.cpp
│   │   ├── NotationRenderer.h
│   │   ├── NotationRenderer.cpp
│   │   ├── RoutingManager.h
│   │   └── RoutingManager.cpp
│   ├── Database/
│   │   ├── DatabaseManager.h
│   │   ├── DatabaseManager.cpp
│   │   └── SchemaDefinitions.h
│   ├── UI/
│   │   ├── MainInterface.h
│   │   ├── MainInterface.cpp
│   │   ├── LayerTabComponent.h
│   │   ├── LayerTabComponent.cpp
│   │   ├── PianoRollComponent.h
│   │   ├── PianoRollComponent.cpp
│   │   ├── NotationComponent.h
│   │   ├── NotationComponent.cpp
│   │   ├── HarmonicAnalysisComponent.h
│   │   └── HarmonicAnalysisComponent.cpp
│   ├── Models/
│   │   ├── Phrase.h
│   │   ├── Stack.h
│   │   ├── Layer.h
│   │   ├── MIDIEvent.h
│   │   └── AnalysisResult.h
│   └── ThirdParty/
│       └── sqlite3.c (fallback if system SQLite not found)
├── Resources/
│   ├── Database/
│   │   └── stratum2_harmonic_data.db
│   └── UI/
│       ├── icons/
│       └── fonts/
└── Scripts/
    ├── build_database.py
    └── deploy.sh
```

## Key Design Decisions

### Modular Architecture
Each core component is self-contained with clear interfaces:
- **ProjectManager**: File I/O, versioning, project state
- **MIDIProcessor**: Real-time MIDI capture and playback
- **TimingAnalyzer**: Post-performance phrase detection
- **LayerStackManager**: Core organizational model
- **HarmonicAnalyzer**: On-demand analysis using database lookups
- **NotationRenderer**: Grand staff display generation
- **RoutingManager**: Multi-target MIDI routing

### Database-First Approach
- Pre-computed SQLite database with comprehensive harmonic analysis data
- Fast lookups instead of real-time calculations
- Confidence scoring for pattern matching
- Optimized indexes for common query patterns

### Three-Phase Workflow Support
- **Improvisation**: Background MIDI capture, minimal UI distraction
- **Composition**: Full interface with layer/stack organization
- **Orchestration**: Routing control focus (future iPad integration)

### JUCE Best Practices
- Proper AU/VST3/Standalone multi-format support
- MIDI effect classification for DAW integration
- Thread-safe audio processing
- Efficient UI updates via timer callbacks
- Resource management and memory safety

## Build Requirements

### Dependencies
- JUCE 7.x or later
- CMake 3.22+
- SQLite3 (system or bundled)
- C++17 compiler

### Platform Support
- macOS 10.15+ (primary target)
- Future: Windows 10+, iPad (USB-C MIDI)

### Integration Targets
- Logic Pro (AU)
- Synthesizer V (project file parsing)
- Standard MIDI export
- Multi-track DAW routing