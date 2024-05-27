Applies to version: 2.2

# Units of play
### Track
A collection of different patterns on the same song. Equivalent to "song" or "beatmap" in other rhythm games. A track includes metadata and any number of patterns.

### Pattern
The unit of gameplay. Each pattern has a name, a difficulty level, and is specific to one control scheme.

### Scan
A section of a pattern, corresponding to one scanline moving from one side of the screen to the other, once. Roughly equivalent to "bar" or "measure" in other rhythm games. At any time during gameplay, 2 scans are visible on the screen.

In a pattern, each scan spans the same number of beats. This number (Beats per scan; BPS in short) is set during pattern setup, and must stay constant throughout the pattern. However BPM can change, so each scan can span different amounts of time.

# Setlists
### Setlist
A named collection of patterns, usually not on the same song. Equivalent to "set" or "course" in other rhythm games. A setlist includes metadata, at least 3 selectable patterns and at least 1 hidden pattern, though the hidden pattern(s) are hidden from the player.

### Selectable pattern
The visible part of a setlist. After the player selects a setlist, they will see a list of all selectable patterns within it, and need to choose 3.

### Hidden pattern
The invisible part of a setlist. After the player completes all 3 selectable patterns they chose, TECHMANIA will choose one from the hidden patterns as a 4th pattern. The player needs to complete this pattern in order to complete the setlist.

### Stage
One playthrough of a setlist consists of 4 patterns, and they are sometimes referred to as stages 1, 2, 3 and 4.

# Note position
### Lane
The vertical position of a note. Lanes are numbered 0 to 63, from top to bottom. The top few lanes are playable lanes; the remaining lanes are hidden lanes. Notes in hidden lanes are hidden notes; these notes are hidden from gameplay, play their keysounds automatically, and do not affect scoring.

### Pulse
The horizontal and temporal position of a note. A pulse is 1/240 of a beat.

# Note components
![Basic note, Hold head, Hold trail, Chain head, Chain path, Chain node](https://imgur.com/WzyFYkE.png)
![Repeat head, Repeat path, Repeat note, Repeat hold trail, Repeat hold note](https://imgur.com/mbjbTZM.png)
![Drag head, Drag curve, Left/right control point, anchor](https://imgur.com/PcbYdpL.png)
