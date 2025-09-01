# Stratum2 Harmonic Analysis Database Design

## Database Schema with Optimized Relationships

### Foundation Tables (Core Music Theory Data)

```sql
-- Primary lookup table for all MIDI notes
CREATE TABLE tonality_frequencies (
    midi_note INTEGER PRIMARY KEY,
    note_name TEXT NOT NULL,
    sharp_flat_status TEXT,
    frequency_hz_440 REAL,
    frequency_hz_432 REAL,
    octave INTEGER,
    range_name TEXT,
    register_weight REAL
);
CREATE INDEX idx_note_name ON tonality_frequencies(note_name);
CREATE INDEX idx_octave ON tonality_frequencies(octave);

-- All possible intervals with acoustic properties
CREATE TABLE tonality_intervals (
    interval_id INTEGER PRIMARY KEY,
    lower_note TEXT,
    upper_note TEXT,
    interval_name TEXT,
    semitones INTEGER,
    frequency_ratio TEXT,
    consonance_level TEXT,
    roughness_index REAL,
    beat_frequency_hz_440 REAL,
    FOREIGN KEY (lower_note) REFERENCES tonality_frequencies(note_name),
    FOREIGN KEY (upper_note) REFERENCES tonality_frequencies(note_name)
);
CREATE INDEX idx_interval_semitones ON tonality_intervals(semitones);
CREATE INDEX idx_consonance ON tonality_intervals(consonance_level);

-- Scale definitions and characteristics
CREATE TABLE tonality_scales (
    scale_id TEXT PRIMARY KEY,
    scale_name TEXT NOT NULL,
    mode_name TEXT,
    scale_degrees TEXT,
    harmonic_tension_level TEXT
);
CREATE INDEX idx_scale_name ON tonality_scales(scale_name);

-- Comprehensive chord database
CREATE TABLE tonality_chords (
    chord_id INTEGER PRIMARY KEY,
    chord_symbol TEXT NOT NULL,
    root_note TEXT,
    bass_note TEXT,
    chord_tones TEXT,
    chord_intervals TEXT,
    chord_quality TEXT,
    chord_scale_relationship TEXT,
    FOREIGN KEY (root_note) REFERENCES tonality_frequencies(note_name),
    FOREIGN KEY (bass_note) REFERENCES tonality_frequencies(note_name)
);
CREATE INDEX idx_chord_symbol ON tonality_chords(chord_symbol);
CREATE INDEX idx_root_note ON tonality_chords(root_note);
CREATE INDEX idx_chord_tones ON tonality_chords(chord_tones);
```

### Analysis Tables (Theory Application)

```sql
-- Functional harmony analysis
CREATE TABLE functional_harmony_analysis (
    function_id INTEGER PRIMARY KEY,
    function_name TEXT,
    scale_degrees_involved TEXT,
    typical_chords TEXT,
    resolution_target TEXT,
    voice_leading_rules TEXT
);

-- Modal characteristics linked to scales
CREATE TABLE modal_harmony_characteristics (
    mode_id INTEGER PRIMARY KEY,
    mode_name TEXT,
    scale_id_fk TEXT,
    characteristic_intervals TEXT,
    avoid_notes TEXT,
    characteristic_chords TEXT,
    FOREIGN KEY (scale_id_fk) REFERENCES tonality_scales(scale_id)
);

-- Emotional analysis of harmonic content
CREATE TABLE emotion_cadences (
    cadence_id INTEGER PRIMARY KEY,
    cadence_type TEXT,
    emotional_archetype TEXT,
    resolution_feeling TEXT,
    dramatic_impact TEXT,
    surprise_level TEXT
);
```

## Pattern Matching Algorithm Design

### Core Analysis Process

```python
# Pseudocode for harmonic analysis algorithm

class HarmonicAnalyzer:
    def __init__(self, database_connection):
        self.db = database_connection
        self.confidence_threshold = 0.7
    
    def analyze_layer_stack(self, midi_notes, timestamp_range):
        """
        Main analysis function - called on keyboard shortcut
        Returns tiered analysis based on user preference level
        """
        
        # Step 1: Convert MIDI to note names
        note_data = self.midi_to_notes(midi_notes)
        
        # Step 2: Identify intervals and chords
        intervals = self.identify_intervals(note_data)
        chords = self.identify_chords(note_data)
        
        # Step 3: Determine analysis confidence
        confidence = self.calculate_confidence(chords, intervals)
        
        if confidence < self.confidence_threshold:
            return None  # No analysis for ambiguous patterns
        
        # Step 4: Generate tiered analysis
        return self.generate_tiered_analysis(chords, intervals, note_data)
    
    def identify_chords(self, note_data):
        """
        Pattern match against chord database
        """
        chord_patterns = []
        
        # Group simultaneous notes
        chord_moments = self.group_simultaneous_notes(note_data)
        
        for moment in chord_moments:
            # Query chord database for pattern matches
            sql = """
            SELECT chord_symbol, chord_quality, chord_scale_relationship
            FROM tonality_chords 
            WHERE chord_tones LIKE ? 
            ORDER BY chord_quality
            """
            
            pattern = self.notes_to_pattern(moment['notes'])
            matches = self.db.execute(sql, (pattern,)).fetchall()
            
            if matches:
                chord_patterns.append({
                    'timestamp': moment['timestamp'],
                    'chord': matches[0],  # Best match
                    'alternatives': matches[1:5]  # Alternative interpretations
                })
        
        return chord_patterns
    
    def generate_tiered_analysis(self, chords, intervals, note_data):
        """
        Generate analysis appropriate to user's theory level
        """
        analysis = {
            'beginner': self.basic_analysis(chords),
            'intermediate': self.functional_analysis(chords, intervals),
            'advanced': self.comprehensive_analysis(chords, intervals, note_data)
        }
        
        return analysis
    
    def basic_analysis(self, chords):
        """
        Simple chord names and qualities
        """
        return {
            'chords': [c['chord']['chord_symbol'] for c in chords],
            'key_suggestion': self.suggest_key(chords)
        }
    
    def functional_analysis(self, chords, intervals):
        """
        Roman numeral analysis, voice leading
        """
        key = self.determine_key_center(chords)
        roman_numerals = self.chords_to_roman_numerals(chords, key)
        
        return {
            'key': key,
            'roman_numerals': roman_numerals,
            'progression_type': self.classify_progression(roman_numerals),
            'voice_leading': self.analyze_voice_leading(intervals)
        }
    
    def comprehensive_analysis(self, chords, intervals, note_data):
        """
        Advanced analysis: modal, post-functional, emotional
        """
        functional = self.functional_analysis(chords, intervals)
        
        # Add advanced analysis
        functional.update({
            'modal_characteristics': self.modal_analysis(note_data),
            'emotional_character': self.emotional_analysis(chords),
            'post_functional_elements': self.post_functional_analysis(chords),
            'voice_leading_sophistication': self.advanced_voice_leading(intervals),
            'registral_analysis': self.analyze_register_distribution(note_data)
        })
        
        return functional
```

### Configurable Analysis Levels

```python
# User preference system for analysis complexity

class AnalysisPreferences:
    LEVELS = {
        'beginner': {
            'show_chord_names': True,
            'show_roman_numerals': False,
            'show_modal_analysis': False,
            'show_emotional_analysis': False,
            'show_voice_leading': False
        },
        'intermediate': {
            'show_chord_names': True,
            'show_roman_numerals': True,
            'show_modal_analysis': False,
            'show_emotional_analysis': True,
            'show_voice_leading': True
        },
        'advanced': {
            'show_chord_names': True,
            'show_roman_numerals': True,
            'show_modal_analysis': True,
            'show_emotional_analysis': True,
            'show_voice_leading': True,
            'show_post_functional': True,
            'show_registral_analysis': True
        }
    }
```

## Timing Analysis Algorithm for Improvisation Phase

### Missing Data Requirements for Phrase Detection

The current time CSV files provide compositional context but lack specific parameters for phrase boundary detection during improvisation. We need to define:

```python
# Additional configuration data needed for TimingAnalysisModule

class PhraseDetectionConfig:
    """Parameters for distinguishing phrases from segments during improvisation"""
    
    # Silence-based phrase boundaries
    SILENCE_THRESHOLDS = {
        'minimum_gap_seconds': 1.5,      # Minimum silence to consider phrase end
        'maximum_gap_seconds': 8.0,      # Beyond this = new improvisation session
        'pedal_release_weight': 2.0,     # Sustain pedal release amplifies silence
    }
    
    # Dynamic change indicators
    DYNAMIC_THRESHOLDS = {
        'velocity_change_threshold': 20,  # MIDI velocity change indicating phrase boundary
        'sudden_dynamic_drop': 0.3,      # Sudden volume reduction factor
        'crescendo_phrase_indicator': 15, # Velocity increase suggesting phrase buildup
    }
    
    # Note density patterns
    DENSITY_PATTERNS = {
        'phrase_minimum_notes': 8,       # Minimum notes to qualify as phrase
        'segment_maximum_notes': 4,      # Below this likely indeterminate segment  
        'note_spacing_variance': 0.5,    # Timing variance threshold for coherence
    }
    
    # Phrase length probabilities
    PHRASE_LENGTH_WEIGHTS = {
        4: 0.15,   # 4-bar phrases (common but brief)
        6: 0.20,   # 6-bar phrases (moderate)
        8: 0.35,   # 8-bar phrases (most common)
        12: 0.20,  # 12-bar phrases (blues, longer forms)
        16: 0.10   # 16-bar phrases (extended forms)
    }

class TimingAnalysisAlgorithm:
    """Post-improvisation analysis for phrase detection"""
    
    def __init__(self, config, time_signature_db, tempo_db):
        self.config = config
        self.time_signatures = time_signature_db
        self.tempo_data = tempo_db
        
    def analyze_improvisation_session(self, midi_events):
        """
        Main phrase detection algorithm
        Returns list of detected phrases and indeterminate segments
        """
        
        # Step 1: Detect tempo and time signature
        detected_tempo = self.detect_tempo(midi_events)
        detected_meter = self.detect_meter(midi_events)
        
        # Step 2: Identify potential phrase boundaries
        boundaries = self.find_phrase_boundaries(midi_events)
        
        # Step 3: Analyze each potential phrase
        phrases = []
        segments = []
        
        for start, end in boundaries:
            section = self.extract_section(midi_events, start, end)
            classification = self.classify_section(section, detected_tempo, detected_meter)
            
            if classification['type'] == 'phrase':
                phrases.append(classification)
            else:
                segments.append(classification)
        
        return {
            'phrases': phrases,
            'segments': segments,
            'session_tempo': detected_tempo,
            'session_meter': detected_meter
        }
    
    def find_phrase_boundaries(self, midi_events):
        """Identify boundaries using silence, dynamics, and pedaling"""
        boundaries = []
        
        # Analyze timing gaps
        silence_boundaries = self.find_silence_gaps(midi_events)
        
        # Analyze sustain pedal patterns
        pedal_boundaries = self.find_pedal_phrases(midi_events)
        
        # Analyze dynamic changes
        dynamic_boundaries = self.find_dynamic_changes(midi_events)
        
        # Combine and weight boundary indicators
        all_boundaries = self.combine_boundary_evidence(
            silence_boundaries, 
            pedal_boundaries, 
            dynamic_boundaries
        )
        
        return all_boundaries
    
    def classify_section(self, section, tempo, meter):
        """Determine if section is structured phrase or indeterminate segment"""
        
        # Analyze rhythmic coherence
        rhythmic_coherence = self.analyze_rhythmic_pattern(section, meter)
        
        # Check phrase length probability
        duration_bars = self.calculate_bar_duration(section, tempo, meter)
        length_probability = self.config.PHRASE_LENGTH_WEIGHTS.get(duration_bars, 0.05)
        
        # Analyze note density and spacing
        note_density = self.analyze_note_density(section)
        
        # Calculate confidence score
        confidence = self.calculate_phrase_confidence(
            rhythmic_coherence, 
            length_probability, 
            note_density
        )
        
        if confidence > 0.6 and duration_bars in [4, 6, 8, 12, 16]:
            return {
                'type': 'phrase',
                'duration_bars': duration_bars,
                'confidence': confidence,
                'rhythmic_character': rhythmic_coherence,
                'start_time': section['start'],
                'end_time': section['end'],
                'midi_data': section['events']
            }
        else:
            return {
                'type': 'segment',
                'duration': section['end'] - section['start'],
                'note_count': len(section['events']),
                'indeterminate_reason': self.identify_segment_characteristics(section)
            }
```

## Grand Staff Notation Implementation

### Database Schema for Notation Elements

```sql
-- Key signature database for staff display
CREATE TABLE notation_key_signatures (
    key_name TEXT PRIMARY KEY,
    key_signature_accidentals TEXT,
    number_of_accidentals INTEGER,
    accidental_type TEXT,
    order_of_accidentals TEXT,
    relative_major_minor TEXT,
    circle_of_fifths_position TEXT,
    scale_degrees TEXT
);

-- Musical symbols and notation elements
CREATE TABLE notation_symbols (
    symbol_id INTEGER PRIMARY KEY,
    symbol_name TEXT,
    symbol_unicode TEXT,
    symbol_ascii TEXT,
    duration_value REAL,
    staff_position TEXT,
    clef_dependency TEXT,
    stem_direction TEXT,
    beam_capability INTEGER,
    visual_description TEXT
);
```

### Layer-to-Staff Algorithm

```python
class GrandStaffRenderer:
    """Convert layer-separated MIDI to grand staff notation"""
    
    def __init__(self, key_signatures_db, symbols_db, harmonic_analysis):
        self.key_signatures = key_signatures_db
        self.symbols = symbols_db
        self.harmonic_analysis = harmonic_analysis
    
    def render_layers_to_staff(self, layer_stack, detected_key):
        """
        Main rendering function - leverages pre-separated layers
        """
        
        # Step 1: Get key signature information
        key_info = self.get_key_signature(detected_key)
        
        # Step 2: Assign clefs based on register analysis
        clef_assignments = self.assign_clefs_by_register(layer_stack)
        
        # Step 3: Convert each layer to staff notation
        staff_voices = []
        for layer_index, layer_data in enumerate(layer_stack):
            voice_notation = self.convert_layer_to_voice(
                layer_data, 
                key_info, 
                clef_assignments[layer_index],
                voice_number=layer_index + 1
            )
            staff_voices.append(voice_notation)
        
        # Step 4: Combine voices onto grand staff
        grand_staff = self.combine_voices_on_staff(staff_voices, key_info)
        
        return grand_staff
    
    def assign_clefs_by_register(self, layer_stack):
        """Use Harmonic_Arrangement_Registers.csv data for clef assignment"""
        clef_assignments = {}
        
        for layer_index, layer_data in enumerate(layer_stack):
            average_midi = self.calculate_average_pitch(layer_data)
            
            # Register-based clef assignment
            if average_midi >= 60:  # Middle C and above
                clef_assignments[layer_index] = 'treble'
            elif average_midi >= 43:  # G2 to B4
                clef_assignments[layer_index] = 'treble'  # or alto for inner voices
            else:  # Below G2
                clef_assignments[layer_index] = 'bass'
        
        return clef_assignments
    
    def convert_layer_to_voice(self, layer_data, key_info, clef, voice_number):
        """Convert single layer MIDI to notation voice"""
        
        voice_notation = {
            'voice_number': voice_number,
            'clef': clef,
            'measures': []
        }
        
        # Group notes by measure based on timing
        measures = self.group_notes_by_measure(layer_data)
        
        for measure_data in measures:
            measure_notation = self.render_measure(
                measure_data, 
                key_info, 
                clef,
                voice_number
            )
            voice_notation['measures'].append(measure_notation)
        
        return voice_notation
    
    def render_measure(self, measure_data, key_info, clef, voice_number):
        """Render individual measure with proper notation conventions"""
        
        measure = {
            'notes': [],
            'rests': [],
            'accidentals': [],
            'dynamics': [],
            'articulations': []
        }
        
        for note_event in measure_data:
            # Convert MIDI note to staff position
            staff_position = self.midi_to_staff_position(
                note_event['midi_note'], 
                clef, 
                key_info
            )
            
            # Determine accidentals needed
            accidental = self.determine_accidental(
                note_event['midi_note'], 
                key_info
            )
            
            # Get note symbol based on duration
            note_symbol = self.get_note_symbol(note_event['duration'])
            
            # Determine stem direction (voice-specific rules)
            stem_direction = self.determine_stem_direction(
                staff_position, 
                voice_number
            )
            
            note_notation = {
                'midi_note': note_event['midi_note'],
                'staff_position': staff_position,
                'duration': note_event['duration'],
                'symbol': note_symbol,
                'accidental': accidental,
                'stem_direction': stem_direction,
                'velocity': note_event['velocity']
            }
            
            measure['notes'].append(note_notation)
        
        return measure
    
    def midi_to_staff_position(self, midi_note, clef, key_info):
        """Convert MIDI note number to staff line/space position"""
        
        # Standard MIDI to staff position conversion
        # Accounts for clef and key signature
        
        if clef == 'treble':
            # Treble clef: line 5 = F5 (MIDI 77)
            middle_c_position = -6  # Middle C below staff
            staff_position = middle_c_position + (midi_note - 60)
        elif clef == 'bass':
            # Bass clef: line 5 = A3 (MIDI 57)  
            middle_c_position = 6   # Middle C above staff
            staff_position = middle_c_position + (midi_note - 60)
        
        return staff_position
    
    def determine_accidental(self, midi_note, key_info):
        """Determine if accidental needed based on key signature"""
        
        # Extract chromatic note name from MIDI number
        chromatic_names = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B']
        note_name = chromatic_names[midi_note % 12]
        
        # Check against key signature accidentals
        key_accidentals = key_info['key_signature_accidentals']
        
        if '#' in note_name and note_name not in key_accidentals:
            return 'sharp'
        elif 'b' in note_name and note_name not in key_accidentals:
            return 'flat'
        elif note_name in key_accidentals:
            return None  # Already in key signature
        else:
            return 'natural'  # Natural sign needed
    
    def determine_stem_direction(self, staff_position, voice_number):
        """Voice-specific stem direction rules"""
        
        # Multi-voice stem direction conventions
        if voice_number == 1:  # Top voice
            return 'up' if staff_position < 3 else 'down'
        elif voice_number == 2:  # Second voice  
            return 'down'  # Usually stems down
        else:
            # Additional voices alternate or follow register
            return 'up' if staff_position < 0 else 'down'

class NotationQuality:
    """Handle advanced notation conventions"""
    
    def __init__(self, musicxml_examples):
        self.examples = musicxml_examples  # For pattern learning
    
    def improve_engraving_quality(self, staff_notation):
        """Apply professional engraving conventions"""
        
        # Beam grouping based on time signature
        self.apply_beam_grouping(staff_notation)
        
        # Collision avoidance between voices
        self.resolve_voice_collisions(staff_notation)
        
        # Spacing optimization
        self.optimize_horizontal_spacing(staff_notation)
        
        return staff_notation
    
    def learn_from_musicxml_examples(self, musicxml_files):
        """Extract engraving patterns from professional scores"""
        
        # Pattern recognition for:
        # - Beam grouping conventions
        # - Voice separation techniques  
        # - Accidental placement
        # - Dynamic positioning
        # - Articulation placement
        
        # This is where MusicXML analysis would inform notation decisions
        pass
```

### MusicXML Pattern Learning Strategy

```python
class MusicXMLPatternExtractor:
    """Extract notation patterns from professional MusicXML scores"""
    
    def analyze_musicxml_collection(self, musicxml_files):
        """Learn professional engraving conventions"""
        
        patterns = {
            'beam_grouping': {},
            'voice_separation': {},
            'accidental_placement': {},
            'stem_directions': {},
            'collision_resolution': {}
        }
        
        for xml_file in musicxml_files:
            # Parse professional notation decisions
            score_patterns = self.extract_patterns(xml_file)
            
            # Accumulate pattern frequencies
            self.merge_patterns(patterns, score_patterns)
        
        # Generate rules based on most common patterns
        notation_rules = self.generate_rules_from_patterns(patterns)
        
        return notation_rules
    
    def extract_patterns(self, musicxml_file):
        """Extract specific engraving decisions from one score"""
        
        # Analyze how professional engravers handle:
        # - Multi-voice works (Bach chorales, piano scores)
        # - Complex rhythms and beam grouping
        # - Accidental placement in chromatic passages
        # - Voice leading visualization
        
        return extracted_patterns
```

The layer separation concept dramatically simplifies grand staff implementation. Instead of complex voice separation algorithms, the system can directly map each layer to a staff voice with appropriate clef assignment based on register analysis.

Your notation CSV data provides the essential lookup tables for key signatures, accidentals, and symbol representations. Combined with MusicXML pattern learning, this could produce professional-quality notation display that enhances the compositional workflow without the complexity of full-featured notation software.

```python
test_cases = [
    # Basic chord identification
    {
        'name': 'Simple C Major Triad',
        'midi_notes': [60, 64, 67],  # C-E-G
        'expected': {
            'chord': 'C',
            'quality': 'major',
            'confidence': 0.95
        }
    },
    
    # Ambiguous harmony (should return None)
    {
        'name': 'Ambiguous Cluster',
        'midi_notes': [60, 61, 62, 63],  # Chromatic cluster
        'expected': None
    },
    
    # Complex jazz chord
    {
        'name': 'C13#11 Chord',
        'midi_notes': [60, 64, 67, 70, 74, 78, 81],
        'expected': {
            'chord': 'C13#11',
            'functional_role': 'dominant',
            'emotional_character': 'sophisticated_tension'
        }
    },
    
    # Modal context
    {
        'name': 'Dorian Progression',
        'chord_sequence': ['Dm7', 'G7', 'Am7', 'Dm7'],
        'expected': {
            'mode': 'D Dorian',
            'modal_characteristics': 'natural_sixth_raised_seventh'
        }
    },
    
    # Voice leading analysis
    {
        'name': 'Smooth Voice Leading',
        'chord_progression': [
            {'chord': 'C', 'voicing': [48, 60, 64, 67]},
            {'chord': 'Am', 'voicing': [48, 60, 64, 69]}
        ],
        'expected': {
            'voice_leading_quality': 'smooth',
            'common_tones': 2,
            'stepwise_motion': True
        }
    }
]
```

## Performance Optimization Strategy

### Query Optimization

```sql
-- Composite indexes for common query patterns
CREATE INDEX idx_chord_lookup ON tonality_chords(root_note, chord_quality, chord_tones);
CREATE INDEX idx_interval_analysis ON tonality_intervals(semitones, consonance_level);
CREATE INDEX idx_scale_chord_relationship ON tonality_chords(chord_scale_relationship);

-- Pre-computed views for frequent analyses
CREATE VIEW chord_scale_compatibility AS
SELECT 
    c.chord_symbol,
    c.root_note,
    s.scale_name,
    s.mode_name
FROM tonality_chords c
JOIN tonality_scales s ON c.chord_scale_relationship LIKE '%' || s.scale_name || '%';
```

### Caching Strategy

```python
# Analysis result caching for responsive UI
class AnalysisCache:
    def __init__(self, max_size=1000):
        self.cache = {}
        self.max_size = max_size
    
    def get_analysis(self, notes_pattern):
        """Check cache before database query"""
        pattern_hash = hash(tuple(sorted(notes_pattern)))
        return self.cache.get(pattern_hash)
    
    def store_analysis(self, notes_pattern, analysis):
        """Cache analysis result"""
        pattern_hash = hash(tuple(sorted(notes_pattern)))
        
        if len(self.cache) >= self.max_size:
            # Remove oldest entry
            oldest_key = next(iter(self.cache))
            del self.cache[oldest_key]
        
        self.cache[pattern_hash] = analysis
```

This database design and algorithm framework provides the foundation for Claude Copilot to implement sophisticated harmonic analysis with the performance characteristics and user experience you're targeting.