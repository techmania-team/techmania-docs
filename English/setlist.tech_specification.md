Applies to version: 2.2

This page explains the specification of TECHMANIA's .tech format for setlists.

If you are working on applications to parse, manipulate or convert .tech files, consider doing it in C# so you can include [Setlist.cs](https://github.com/techmania-team/techmania/blob/2.2/TECHMANIA/Assets/Scripts/Serializable/Setlist.cs) and call `Setlist.Serialize` and `Setlist.Deserialize`, making it unnecessary to read the following.

# setlist.tech
```
{
    "version": "1",
    "setlistMetadata": {
        "guid": <guid>,
		"title": <title>,
		"description": <description>,
		"eyecatchImage": <filename of eyecatch image>,
		"backImage": <filename of background image>,
		"controlScheme": <control scheme>
    },
    "selectablePatterns": [
        <selectable pattern 1>,
        <selectable pattern 2>,
        <...>,
        <selectable pattern n>
    ],
    "hiddenPatterns": [
        <hidden pattern 1>,
        <hidden pattern 2>,
        <...>,
        <hidden pattern n>,
    ]
}
```

* `version` must be "1".
* `guid` is a unique identifier of each setlist. Setlist records rely on this identifier to indicate the setlist they are on. Therefore, once a setlist is created, its GUID should never change. TECHMANIA generates a GUID when you create a new setlist, but if you need to generate your own, you can use any GUID generator, such as [this one](https://www.guidgenerator.com/online-guid-generator.aspx).
* `version`, `guid`, `title`, `description`, `eyecatchImage`, `backImage` are strings, so you need to write quotation marks around the values. `controlScheme` is a number, no quotation marks needed.
* `controlScheme` is 0 for Touch, 1 for Keys, and 2 for KM.
* All filenames in `setlist.tech` are without folders, but with extensions.
* Make sure to write a comma at the end of every line, except for the last line between any pair of parentheses.

# Selectable pattern

```
		{
			"trackTitle": <track title>,
			"trackGuid": <track guid>,
			"patternName": <pattern name>,
			"patternLevel": <pattern level>,
			"patternPlayableLanes": <pattern playable lanes>,
			"patternGuid": <pattern guid>
		},
```

* `trackTitle`, `trackGuid`, `patternName`, `patternGuid` are strings; `patternLevel` and `patternPlayableLanes` are integers.
* Only `trackGuid` and `patternGuid` actually matter in allowing TECHMANIA to find the pattern on the player's device; all other fields only serve informational purposes. In the default theme, these fields are used to tell the user about any pattern(s) they are missing.

# Hidden pattern

```
		{
			"reference": {
				"trackTitle": <track title>,
                "trackGuid": <track guid>,
                "patternName": <pattern name>,
                "patternLevel": <pattern level>,
                "patternPlayableLanes": <pattern playable lanes>,
                "patternGuid": <pattern guid>
			},
			"criteriaType": <criteria type>,
			"criteriaDirection": <criteria direction>,
			"criteriaValue": <criteria value>
		},
```

* All fields in `reference` follow the same format for selectable patterns.
* `criteriaType`, `criteriaDirection` and `criteriaValue` are all integers. They correspond to the 2 dropdowns and 1 text field under a hidden pattern that's not the last one; check the setlist editor for explanations. On the last hidden pattern, these fields still exist but are not used.
* `criteriaType` is:
    * 0 for index
    * 1 for level
    * 2 for HP
    * 3 for score
    * 4 for combo
    * 5 for max combo
    * 6 for D100
* `criteriaDirection` is 0 for "<", and 1 for ">".
