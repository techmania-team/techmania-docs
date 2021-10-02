Applies to version: 1.4

Here are all the commands, special notes, attributes and tracks that are supported.

## Track mapping
* Tracks 0-3 (P1 visible) are mapped to lanes 0-3.
* Tracks 4-7 (P1 special) modify notes in lanes 0-3.
* Tracks 8-15 (P2 visible & special) are ignored. The converter will warn if any event is found in these tracks.
* Tracks 16 and onward are mapped to lanes 4 and onward. If this results in more than 64 lanes, the conversion fails.

## Commands
* Command 1 (note): supported.
* Command 2 (volume): supported.
* Command 3 (tempo): supported.
* Command 4 (beat): not supported, and silently ignored.
* Other commands will cause a warning.

## Note attributes (not in special tracks)
* 0, 5, 6, 10, 11, 12: supported, converted to corresponding TECHMANIA note types. Drag notes will use B-splines.
* 100 (video start): the first note with this attribute becomes the converted pattern's BGA offset.
* Other attributes will cause a warning.

## Note attributes (in special tracks)
* For special notes with attribute 0 and 4 tracks below a visible note, that visible note becomes an end-of-scan note.
* For special notes 4 tracks below a drag note (attribute 0 with duration >6), they are converted to anchors (see next section).
* Other special notes will cause a warning.

## Drag anchors
If a special note is `p` pulses away from the previous special note or note head (whichever is closer), its attribute is `attr`, and the pattern is in `l` lanes, then in the converted drag note anchor, the `lane` value is:
* `(attr - 60) * p * l / 43200`, if scroll speed is 1
* `(attr - 60) * p * l / 21600`, if scroll speed is 2

Because of the difference in formulas, it's critical that the converter finds the correct scroll speed for each pattern.