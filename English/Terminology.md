Applies to version: 2.0

# Units of play
### Track
A collection of different patterns on the same song. Equivalent to "song" or "beatmap" in other rhythm games. A track includes metadata and any number of patterns.

### Pattern
The unit of gameplay. Each pattern has a name, a difficulty level, and is specific to one control scheme.

### Scan
A section of a pattern, corresponding to one scanline moving from one side of the screen to the other, once. Roughly equivalent to "bar" or "measure" in other rhythm games. At any time during gameplay, 2 scans are visible on the screen.

In a pattern, each scan spans the same number of beats. This number (Beats per scan; BPS in short) is set during pattern setup, and must stay constant throughout the pattern. However BPM can change, so each scan can span different amounts of time.

# Note position
### Lane
The vertical position of a note. Lanes are numbered 0 to 63, from top to bottom. The top few lanes are playable lanes; the remaining lanes are hidden lanes. Notes in hidden lanes are hidden notes; these notes are hidden from gameplay, play their keysounds automatically, and do not affect scoring.

### Pulse
The horizontal and temporal position of a note. A pulse is 1/240 of a beat.

# Note components
![Basic note, Hold head, Hold trail, Chain head, Chain path, Chain node](https://imgur.com/WzyFYkE.png)
![Repeat head, Repeat path, Repeat note, Repeat hold trail, Repeat hold note](https://imgur.com/mbjbTZM.png)
![Drag head, Drag curve, Left/right control point, anchor](https://imgur.com/PcbYdpL.png)
