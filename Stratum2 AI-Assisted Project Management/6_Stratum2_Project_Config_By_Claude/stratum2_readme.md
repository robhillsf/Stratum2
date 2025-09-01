# Stratum2

A three-dimensional composition tool that organizes harmonic material in layers, stacks, and phrases instead of traditional timeline-based approaches.

## Core Innovation

Stratum2 enables composers to maintain harmonic coherence while introducing variation across repetition through a unique three-dimensional organizational model:

- **Phrase**: Harmonic container representing a musical idea (4-16 bars)
- **Stack**: Grouping mechanism for related layers, enables variation through repetition  
- **Layer**: Individual musical line, interval, or harmonic figuration

## Three-Phase Workflow

### Phase 1: Improvisation (Flow State)
- Silent background MIDI capture during keyboard improvisation
- Post-performance timing analysis and phrase detection
- No real-time harmonic analysis to preserve creative flow
- MIDI-mapped phrase auditioning controls (A0/A#0/B0)

### Phase 2: Composition & Arrangement (Focused Attention)
- Layer/stack/phrase organizational interface
- Tabbed layer navigation with visual transparency
- On-demand harmonic analysis (keyboard shortcut triggered)
- Dual view: MIDI piano roll + grand staff notation
- Multi-target MIDI routing to DAW tracks

### Phase 3: Orchestration (Return to Flow State)
- iPad remote control via USB-C MIDI (future feature)
- Real-time routing adjustments from monitoring position
- Sound field shaping through routing changes

## Features

### Harmonic Analysis
- Database-driven analysis with pre-computed lookup tables
- Confidence scoring eliminates ambiguous results
- Configurable complexity levels (beginner/intermediate/advanced)
- Comprehensive theory coverage: functional harmony, modal analysis, voice leading

### Educational Support
- 571 music theory terms with contextual definitions
- 31 song form templates for accelerated composition
- Progressive learning through configurable analysis depth

### Integration
- AU/VST3/Standalone plugin formats
- DAW synchronization and multi-track MIDI routing
- Synthesizer V project file parsing
- Standard MIDI export

## Build Requirements

### Dependencies
- JUCE 7.x or later
- CMake 3.22+
- SQLite3 (system or bundled)
- C++17 compiler

### macOS Build
```bash
# Install dependencies
brew install cmake sqlite3 juce

# Clone and build
git clone <repository-url> Stratum2
cd Stratum2
mkdir build && cd build
cmake ..
make -j$(nproc)
```

### Project Setup
1. Ensure JUCE is installed and findable by CMake
2. Place your 32 CSV data files in `Resources/CSV/` 
3. Run the database builder: `python Scripts/build_database.py`
4. Build the project using CMake

## Architecture

### Core Components
- **ProjectManager**: File I/O, versioning, project state
- **MIDIProcessor**: Real-time MIDI capture and playback  
- **TimingAnalyzer**: Post-performance phrase boundary detection
- **LayerStackManager**: Three-dimensional organizational model
- **HarmonicAnalyzer**: On-demand database-driven analysis
- **NotationRenderer**: Grand staff display generation
- **RoutingManager**: Multi-target MIDI routing

### Database Design
Pre-computed SQLite database with optimized lookups for:
- 88 MIDI note frequency mappings (440Hz/432Hz)
- 1,097 interval relationships with acoustic properties
- 768 comprehensive chord definitions
- 27 scale/modal characteristics
- Functional harmony, voice leading, and emotional analysis patterns

## Development Status

This is the initial project foundation. Core implementation targets:

1. ✅ Project structure and build system
2. ⏳ Basic MIDI capture and layer organization
3. ⏳ Database integration and harmonic analysis
4. ⏳ UI implementation with tabbed layers
5. ⏳ Grand staff notation display
6. ⏳ DAW integration and routing
7. ⏳ iPad remote control (future)

## License

[Your chosen license]

## Contributing

[Contributing guidelines if accepting contributions]