# Stratum2 Vision Document

## Core Purpose

Stratum2 is a composition tool that solves fundamental frustrations with current music software by organizing harmonic material in three dimensions instead of the typical two-dimensional timeline approach. The app enables composers to maintain harmonic coherence while introducing variation across repetition, preventing the loss of creative control that occurs when navigating endless DAW timelines.

## Three-Phase Creative Workflow

### Phase 1: Improvisation (Flow State)
The composer improvises on a MIDI keyboard while the app captures performance data silently in the background. When improvisation ends (configurable silence threshold), the app analyzes timing patterns and distinguishes structured "phrases" (4/6/8/12/16 bars) from indeterminate "segments." No harmonic analysis occurs here - this phase preserves creative flow by focusing only on timing and phrase boundary detection.

**Key Features:**
- Silent background MIDI capture during improvisation
- Post-performance timing analysis and phrase detection  
- Visual presentation of detected phrases in scrollable column
- Phrase auditioning with MIDI-mapped controls (A0 play/stop, B0 save, A#0 scroll)
- No distracting real-time feedback during performance

### Phase 2: Composition & Arrangement (Focused Attention) 
The composer works with saved phrases using the core organizational innovation: the layer/stack/phrase model. Each phrase can contain multiple stacks, each stack can contain multiple layers (max 8 each). Layers function like transparent sheets in MS Visio, revealing the MIDI piano roll grid beneath. Stacks act as containers that group layers for arrangement linking.

**Key Features:**
- Tabbed layer interface for easy navigation between voices
- Visual layer transparency with color coding
- Stack linking for sequential arrangement
- On-demand harmonic analysis triggered by keyboard shortcut
- Configurable analysis levels (beginner/intermediate/advanced)
- Dual view: MIDI piano roll + grand staff notation display
- Multi-target MIDI routing (single layer to multiple DAW tracks)

### Phase 3: Orchestration (Return to Flow State)
The composer uses an iPad as a remote control via USB-C MIDI connection to adjust routing assignments while sitting in the studio monitoring sweet spot. This phase focuses on shaping the sound field through routing changes without the visual distraction of the composition interface.

**Key Features:**
- iPad routing grid interface for real-time MIDI routing control
- USB-C MIDI connection to desktop app (no cloud sync complexity)
- Distance listening from monitoring position
- Routing preset management
- Real-time routing changes during playback

## Core Innovation: Layer/Stack/Phrase Organization

**Phrase:** Harmonic container representing a musical idea (4-16 bars)
**Stack:** Grouping mechanism for related layers, enables variation through repetition
**Layer:** Individual musical line, interval, or harmonic figuration

This three-dimensional organization allows composers to:
- Maintain harmonic coherence across variations
- Create sophisticated arrangements without timeline navigation
- Introduce controlled variation without losing musical context
- Work with phrase-level concepts rather than linear timelines

## Technical Architecture

### Modular Design
- **ProjectManagementModule:** File I/O, versioning, cross-phase coordination
- **MIDIProcessingModule:** Real-time capture, playback, routing
- **TimingAnalysisModule:** Post-performance phrase detection
- **LayerStackModule:** Core organizational model with tabbed interface
- **HarmonicAnalysisModule:** On-demand analysis using pre-computed database lookups
- **NotationModule:** Grand staff display derived from MIDI data
- **RoutingModule:** Multi-target MIDI routing and DAW integration
- **iPadRemoteModule:** USB-C MIDI controller interface

### Database-Driven Harmonic Analysis
Pre-computed lookup tables eliminate real-time calculation overhead:
- 88 frequency mappings for MIDI notes (supporting both 440Hz and 432Hz tuning)
- 1,097 interval relationships with acoustic properties and consonance data
- 768 comprehensive chord definitions including extensions and alterations
- 27 scale and modal characteristics with cultural associations
- Comprehensive harmonic analysis: 27 functional harmony patterns, 26 chord substitutions, 25 modulation techniques
- Modal analysis with characteristic intervals and voice leading tendencies
- Post-functional harmony covering 24 contemporary techniques
- Emotional analysis with 13 psychological phenomena and 27 cadence archetypes
- Voice leading analysis with 48 voicing types and 8 registral ranges

Analysis is triggered on-demand (keyboard shortcut) rather than automatically, preserving creative flow and preventing cognitive overload. Confidence scoring ensures ambiguous patterns return no analysis rather than uncertain results.

## Platform Strategy

**Target Platforms:** Mac desktop + iPad (USB-C connection)
**Development Framework:** JUCE for cross-platform audio application development
**Database:** SQLite with optimized indexing for fast harmonic lookups
**Integration:** DAW synchronization, Synthesizer V project file parsing for vocal reference

## User Experience Principles

### Flow State Preservation
- No automatic harmonic analysis during creative phases
- Minimal UI during improvisation capture
- On-demand theoretical feedback when specifically requested
- Keyboard shortcuts for all primary functions

### Educational Support & Song Form Templates
The application includes comprehensive learning support and composition acceleration features:
- **Glossary Integration:** 571 music theory terms with definitions, examples, and related concepts accessible via rollover text and contextual help
- **Song Form Templates:** 31 pre-configured structural templates (sonata form, AABA pop songs, blues progressions, etc.) that create linked stack arrangements automatically
- **Configurable Analysis Levels:** Beginner (chord names, key suggestions), Intermediate (Roman numeral analysis, voice leading), Advanced (modal analysis, post-functional harmony, emotional characterization)
- **Genre Context:** 56 musical genres with associated harmonic features, rhythmic characteristics, and arrangement patterns

This educational framework allows users to learn theory concepts within their creative workflow rather than requiring separate study, while song form templates accelerate composition for users working within established structures.

### Meeting Users Where They Are
The app serves multiple use cases and experience levels:
- **Composers:** Complete three-phase workflow for sophisticated harmonic arrangements
- **Improvisers:** Phrase capture and organization tools without requiring complex arrangement features
- **Producers:** Advanced routing control and MIDI management with optional compositional features
- **Educators:** Comprehensive harmonic analysis and 571-term glossary for theory instruction
- **Students:** Progressive learning through configurable analysis complexity and contextual definitions

This multi-user approach ensures the application provides value whether used for professional composition, creative exploration, production workflow management, or educational purposes. Users can engage with features at their comfort level while having room to grow into more sophisticated capabilities.

## Integration Features

### DAW Connectivity
- Transport synchronization and timeline coordination
- Multi-track MIDI routing from individual layers
- Plugin parameter automation through layer assignments
- Project export in standard MIDI format

### Synthesizer V Integration
- Parse Synth V project files for melody and lyric reference
- Display vocal lines as ghost notation during harmonic composition
- Timeline synchronization between app phrases and vocal sections
- Eliminates need to switch between composition app and Synth V interface

## Development Approach

### Modular Implementation
Each phase can be developed and tested independently:
1. Core layer/stack organization (composition phase foundation)
2. Basic improvisation capture (without harmonic analysis)
3. On-demand harmonic analysis system
4. iPad routing controller
5. Advanced integration features

### Database-First Strategy
Comprehensive harmonic analysis database provides:
- Fast pattern matching instead of algorithmic calculation
- Consistent theoretical foundation across all analysis
- Extensible framework for additional harmonic concepts
- Reliable confidence scoring for ambiguous patterns

## Success Criteria

The app succeeds if it:
- Eliminates frustration with traditional DAW timeline navigation
- Enables confident harmonic variation without losing musical context
- Preserves creative flow across all three phases
- Provides theoretical insight without overwhelming users
- Integrates seamlessly with existing studio workflows

This vision represents a genuinely three-dimensional approach to composition software, where the organizational model serves musical thinking rather than forcing musical concepts into software constraints.