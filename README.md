The original ABC files were sourced from the [Nottingham Music Database](http://abc.sourceforge.net/NMD/), and are tracked here untouched.

## Technical cleaning specifications

Here we list the manual and programmatic modifications we made to the ABC files to be able to convert them to MIDI.

### ABC files

Running a `diff` command between a file in the original folder and its corresponding cleaned version will highlight all the changes we made to the plain text ABC.

#### Chord notation
We made the chord notation more consistent and easily parsable by:

1. Removing uninterpretable symbols (e.g. `"/@<.5A7"`)
2. Using one single format for diminished/augmented/over-note notation (`"Cd"` for diminished, `"Ca"` for augmented and `"C/e"` for a C chord over E)

#### Repeats
We strived to make the repeat notation more machine-readable by:

1. Adding beginning-of-repeat symbols (`|:`) whenever their position might be programmatically ambiguous
2. Uniforming the first and second time bar notation (some pieces were using only the numbers `1` and `2`, others the symbols `[1` and `[2`; we opted for the second)
3. Adding double bars `||` at the end of all second time bars, to make it more transparent which notes are part of a repetition structure and which are not.

#### Part notation
We expanded the usage of the part name notation, and used it to encode for other score notations that are not easily machine-interpretable:

1. We modified slightly the usage of the `P` metadata, allowing us to distinguish easily between a single part name (`P`) and the piece part playing sequence (new piece metadata label `Y`), which in the originals are both under the same tag `P`
2. Substituted all notations of "Da Capo al Segno" or "Dal Segno" with a corresponding part subdivision and playing repetition.

#### Simplification choices
The chordal parts of a few pieces contain what can be considered a _walking bass_ sequence of notes intermixed with the chords themselves. 
We decided to remove these extra notes from the cleaned ABC files for two reasons: 

1. The chords are significantly easier to interpret without them; 
2. There are only very few pieces that have this feature, making it of no interest for the purpose of learning algorithms.

We also removed the lyrics from the few pieces that contained them for the same reason.

#### Generic cleaning

Some pieces contained bars of the wrong duration because of an inconsistent number of notes or note lengths. 
In most cases we tried to fix these inconsistencies by filling the bar or reducing the duration of the notes in the way that we thought made the most musical sense, but in a couple of cases we opted for simply removing the offending pieces (e.g. the Pachelbel’s Canon renditions).

### MIDI conversion

Some ABC pieces have two sequences of alternative chords in a repeat, so that when playing the melody the second time the chord sequence is different. 
This is valuable musical information, so we did not remove it from the ABC files, but we decided to ignore it in the MIDI conversion to simplify the parsing operation, so that all repeats always use the first chord of each two-chord alternate pair.

-----
This repository is released under the GNU GPLv3 license.
