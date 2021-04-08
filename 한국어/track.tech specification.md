기준 버전: 0.4

TECHMANIA에서 사용하는 Track.tech 파일의 형식을 소개하는 문서입니다.

이 파일을 분석하여 이용하거나 다른 형식으로 변환하는 프로그램을 만들려면, C#을 이용하는 것을 권장합니다. 프로젝트에 [Track.cs](https://github.com/techmania-team/techmania/blob/master/TECHMANIA/Assets/Scripts/Serializable/Track.cs) 파일을 추가하고, `TrackBase.Serialize` 와 `TrackBase.Deserialize` 메소드를 호출하십시오. 아래 내용을 읽을 필요 없이 TECH 파일을 이용할 수 있습니다.

# Track.tech
```
{
	"version": "2",
	"trackMetadata": {
		"guid": <GUID>,
		"title": <곡 제목>,
		"artist": <아티스트>,
		"genre": <장르>,
		"additionalCredits": <곡의 추가 정보>,
		"eyecatchImage": <미리보기 이미지 파일>,
		"previewTrack": <미리듣기 음악 파일>,
		"previewStartTime": <프리뷰 시작 시점>,
		"previewEndTime": <프리뷰 종료 시점>
	},
	"patterns": [
		<패턴 1>,
		<패턴 2>,
		<...>,
		<패턴 n>
	]
}
```

* `version`의 값은 항상 "2"입니다.
* `guid`는 각 노래의 고유한 ID입니다. 후속 버전에서 곡별 설정 지원을 위해 이 GUID를 사용할 수 있습니다. 따라서 노래 데이터를 생성하면, 이 GUID가 바뀌어서는 안 됩니다. TECHMANIA는 곡 데이터를 생성할 때 GUID를 만듭니다. 필요하다면 [GUID 생성기](https://www.guidgenerator.com/online-guid-generator.aspx)를 직접 이용할 수 있습니다.
* `version`, `guid`, `title`, `artist`, `genre`, `additionalCredits`, `eyecatchImage`, `previewTrack`는 문자열 필드입니다. 반드시 값을 큰따옴표 안에 채우십시오.
* `previewStartTime` and `previewEndTime`는 실수 필드입니다.
* `track.tech` 안에 들어가는 파일명은 디렉토리 경로를 포함하지 않고, 확장자를 포함합니다. 또한, 파일명은 대소문자를 구분합니다.
* 중괄호 한 쌍 안에서는 마지막 줄을 제외하고 요소를 구분하는 모든 줄의 맨 끝에 쉼표(,)가 필요합니다.

# Pattern
```
		{
			"patternMetadata": {
				"guid": <guid>,
				"patternName": <pattern name>,
				"level": <level>,
				"controlScheme": <control scheme>,
				"lanes": <lanes>,
				"author": <pattern author>,
				"backingTrack": <filename of backing track>,
				"backImage": <filename of background image>,
				"bga": <filename of BGA>,
				"bgaOffset": <BGA offset>,
				"firstBeatOffset": <first beat offset>,
				"initBpm": <initial BPM>,
				"bps": <Beats Per Scan>
			},
			"bpmEvents": [
				<BPM event 1>,
				<...>,
				<BPM event n>
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
* `guid`, `patternName`, `author`, `backingTrack`, `backImage`, `bga` are strings; `level`, `controlScheme`, `lanes`, `bps` are integers; `bgaOffset`, `firstBeatOffset`, `initBpm` are floating point numbers (non-integers allowed).
* `controlScheme` is 0 for Touch, 1 for Keys, and 2 for KM.
* `lanes` is currently unused.
* `backingTrack`, `backImage`, and `bga` are optional. Write "" if there is none.
* Check the tooltips in the editor for explanations on BGA offset, first beat offset and Beats Per Scan.

# BPM event

```
				{
					"pulse": <pulse>,
					"bpm": <bpm>
				}
```

* `pulse` is an integer; `bpm` is a floating point number.
* `pulse` describes the position of this event. A pulse is 1/240 of a beat.

# Note

The `packedNotes` section covers all note types without a duration: Basic, Chain Head, Chain Node, Repeat Head, and Repeat. Each note is represented as a string in one of the following two formats:
* `<type>|<pulse>|<lane>|<keysound>`
* `E|<type>|<pulse>|<lane>|<volume>|<pan>|<end-of-scan>|<keysound>`

Which format to use depends on whether the note has default values on volume, pan and end-of-scan:
* Volume ranges from 0 to 1, default is 1
* Pan ranges from -1 to 1, default is 0
* End-of-scan can be 0 (no) or 1 (yes), default is 0

If any of these values are different from default, that note will use the 2nd format. "E" stands for "Extended".

Other notes:
* `type` is one of `Basic`, `ChainHead`, `ChainNode`, `RepeatHead` and `Repeat`.
* `pulse`, `lane` and `end-of-scan` are integers; `volume` and `pan` are floating point numbers.
* Lanes are numbered 0 to 63 from top to bottom.
* `keysound` is optional. Leave this part empty if a note has no keysound, but the `|` before it cannot be omitted.

# Hold note

The `packedHoldNotes` section covers notes of type Hold, Repeat Head Hold and Repeat Hold. Each note is represented as a string in one of the following two formats:
* `<type>|<lane>|<pulse>|<duration>|<keysound>`
* `E|<type>|<lane>|<pulse>|<duration>|<volume>|<pan>|<end-of-scan>|<keysound>`

Similar to the previous section, a note will use the 2nd format if its volume, pan or end-of-scan value is different from default.

Notice that `pulse` and `lane` are reversed in this section. This is a bug from 0.1, and unfortunately there is no way to fix it without breaking every single pattern ever created.

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
* `E|Drag|<pulse>|<lane>|<volume>|<pan>|<end-of-scan>|<keysound>`

Similar to the previous section, a note will use the 2nd format if its volume, pan or end-of-scan value is different from default. Drag notes should never be marked as end-of-scan, though.

Each drag note must contain at least 2 nodes. Each drag node consists of 1 anchor and 2 control points, and is represented as a string in the following format:

`<anchor pulse>|<anchor lane>|<left control point pulse>|<left control point lane>|<right control point pulse>|<right control point lane>`

All values are floating point numbers. The anchor's pulse and lane is relative to the note head; the control points' pulse and lane are relative to the anchor. Additionally, the first node is always located at the note head, so its pulse and lane are both 0, and its left control point is ignored; the right control point on the last node is also ignored.
