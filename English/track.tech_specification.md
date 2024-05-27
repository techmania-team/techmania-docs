Applies to version: 2.2

This page explains the specification of TECHMANIA's .tech format for tracks.

If you are working on applications to parse, manipulate or convert .tech files, consider doing it in C# so you can include [Track.cs](https://github.com/techmania-team/techmania/blob/master/TECHMANIA/Assets/Scripts/Serializable/Track.cs) and call `Track.Serialize` and `Track.Deserialize`, making it unnecessary to read the following.

# track.tech
```
{
	"version": "3",
	"trackMetadata": {
		"guid": <guid>,
		"title": <title>,
		"artist": <artist>,
		"genre": <genre>,
		"additionalCredits": <additional credits>,
		"eyecatchImage": <filename of eyecatch image>,
		"previewTrack": <filename of preview track>,
		"previewStartTime": <preview start time>,
		"previewEndTime": <preview end time>,
		"previewBga", <filename of preview BGA>,
		"autoOrderPatterns": <true or false>
	},
	"patterns": [
		<pattern 1>,
		<pattern 2>,
		<...>,
		<pattern n>
	]
}
```

* `version` must be "3".
* `guid` is a unique identifier of each track. TECHMANIA uses this identifier to save per-track options, such as background brightness. Therefore, once a track is created, its GUID should never change. TECHMANIA generates a GUID when you create a new track, but if you need to generate your own, you can use any GUID Generator, such as [this one](https://www.guidgenerator.com/online-guid-generator.aspx).
* `version`, `guid`, `title`, `artist`, `genre`, `additionalCredits`, `eyecatchImage`, `previewTrack`, `previewBga` are strings, so you need to write quotation marks around the values. `previewStartTime` and `previewEndTime` are numbers, no quotation marks needed.
* All filenames in `track.tech` are without folders, but with extensions.
* Make sure to write a comma at the end of every line, except for the last line between any pair of parentheses.

# Pattern
```
		{
			"patternMetadata": {
				"guid": <guid>,
				"patternName": <pattern name>,
				"level": <level>,
				"controlScheme": <control scheme>,
				"playableLanes": <playable lanes>,
				"author": <pattern author>,
				"backingTrack": <filename of backing track>,
				"backImage": <filename of background image>,
				"bga": <filename of BGA>,
				"bgaOffset": <BGA offset>,
				"waitForEndOfBga": <wait for end of BGA>,
				"playBgaOnLoop": <play BGA on loop>,
				"firstBeatOffset": <first beat offset>,
				"initBpm": <initial BPM>,
				"bps": <Beats Per Scan>
			},
			"legacyRulesetOverride": {
				"timeWindows": <time windows>,
				"hpDeltaBasic": <HP delta>,
				"hpDeltaChain": <HP delta>,
				"hpDeltaHold": <HP delta>,
				"hpDeltaDrag": <HP delta>,
				"hpDeltaRepeat": <HP delta>,
				"hpDeltaBasicDuringFever": <HP delta>,
				"hpDeltaChainDuringFever": <HP delta>,
				"hpDeltaHoldDuringFever": <HP delta>,
				"hpDeltaDragDuringFever": <HP delta>,
				"hpDeltaRepeatDuringFever": <HP delta>
			},
			"bpmEvents": [
				<BPM event 1>,
				<...>,
				<BPM event n>
			],
			"timeStops": [
				<time stop 1>,
				<...>,
				<time stop n>
			],
			"packedNotes": [
				<note 1>,
				<...>,
				<note n>
			],
			"packedHoldNotes": [
				<hold note 1>,
				<...>,
				<hold note n>
			],
			"packedDragNotes": [
				<drag note 1>,
				<...>,
				<drag note n>
			]
		}
```

* `guid` is, again, a unique identifier for this pattern.
* `guid`, `patternName`, `author`, `backingTrack`, `backImage`, `bga` are strings; `level`, `controlScheme`, `playableLanes`, `bps` are integers; `bgaOffset`, `firstBeatOffset`, `initBpm` are floating point numbers (non-integers allowed).
* `controlScheme` is 0 for Touch, 1 for Keys, and 2 for KM.
* `backingTrack`, `backImage`, and `bga` are optional. Write "" if there is none.
* `waitForEndOfBga` and `playBgaOnLoop` are `true` or `false`.
* Check the tooltips in the editor for explanations on BGA offset, wait for end of BGA, first beat offset and Beats Per Scan.
* When using the legacy ruleset, the time windows and HP deltas (if existing and not empty) in `legacyRulesetOverride` will override the corresponding values in the legacy ruleset. Refer to the [Ruleset](Rulesets.md) page for an explanation of these parameters.
	* There is no in-game UI for this feature.

# BPM event

```
				{
					"pulse": <pulse>,
					"bpm": <bpm>
				}
```

* `pulse` is an integer; `bpm` is a floating point number.
* `pulse` describes the position of this event. A pulse is 1/240 of a beat.

# Time stop

```
				{
					"pulse": <pulse>,
					"duration": <duration>
				},
```

* `pulse` and `duration` are both in integers.
* `pulse` describes the starting position of this event.
* `duration` describes the duration of this time stop, in pulses.

# Note

The `packedNotes` section covers all note types without a duration: Basic, Chain Head, Chain Node, Repeat Head, and Repeat. Each note is represented as a string in one of the following two formats:
* `<type>|<pulse>|<lane>|<keysound>`
* `E|<type>|<pulse>|<lane>|<volume>|<pan>|<end-of-scan>|<keysound>`

Which format to use depends on whether the note has default values on volume, pan and end-of-scan:
* Volume ranges from 0 to 100, default is 100
* Pan ranges from -100 to 100, default is 0
* End-of-scan can be 0 (no) or 1 (yes), default is 0

If any of these values are different from default, that note will use the 2nd format. "E" stands for "Extended".

Other notes:
* `type` is one of `Basic`, `ChainHead`, `ChainNode`, `RepeatHead` and `Repeat`.
* `pulse`, `lane`, `volume`, `pan` and `end-of-scan` are integers.
* Lanes are numbered 0 to 63 from top to bottom.
* `keysound` is optional. Leave this part empty if a note has no keysound, but the `|` before it cannot be omitted.

# Hold note

The `packedHoldNotes` section covers notes of type Hold, Repeat Head Hold and Repeat Hold. Each note is represented as a string in one of the following two formats:
* `<type>|<pulse>|<lane>|<duration>|<keysound>`
* `E|<type>|<pulse>|<lane>|<duration>|<volume>|<pan>|<end-of-scan>|<keysound>`

Similar to the previous section, a note will use the 2nd format if its volume, pan or end-of-scan value is different from default.

Other notes:
* `duration` is in integer pulses.

# Drag note

This section covers all drag notes. Each drag note is represented as the following structure:

```
				{
					"packedNote": <drag note>,
					"packedNodes": [
						<drag node 1>,
						<...>,
						<drag node n>
					]
				}
```

The `packedNote` part is a string in one of the two following formats:
* `Drag|<pulse>|<lane>|<keysound>`
* `E|Drag|<pulse>|<lane>|<volume>|<pan>|<curve type>|<keysound>`

`curve type` is `0` for Bézier, `1` for B-spline, and the default is `0`.

Similar to the previous section, a note will use the 2nd format if its volume, pan or curve type value is different from default.

Each drag note must contain at least 2 nodes. Each drag node consists of 1 anchor and 2 control points, and is represented as a string in the following format:

`<anchor pulse>|<anchor lane>|<left control point pulse>|<left control point lane>|<right control point pulse>|<right control point lane>`

All values are floating point numbers. The anchor's pulse and lane is relative to the note head; the control points' pulse and lane are relative to the anchor. Additionally, the first node is always located at the note head, so its pulse and lane are both 0, and its left control point is ignored; the right control point on the last node is also ignored.
