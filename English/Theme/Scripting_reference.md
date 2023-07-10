# TECHMANIA Theme API scripting reference

Applies to API version: 1

When reading on Github, you can click the menu button to the top right to reveal a table of contents.

# `getApi`

`getApi` is the entry point to TECHMANIA Theme API. It is also the only thing that the Theme API adds to the global scope. It returns a table with the following values:

|Key|Value|
|--|--|
|`tm`|An object of type [`ThemeApi.Techmania`](#class-themeapitechmania)|
|`net`|A Lua table exposing some .Net types|
|`unity`|A Lua table exposing some Unity types|

The rest of this document assumes you have promoted these values to the global scope:

```
api = getApi(1)
tm = api.tm
net = api.net
unity = api.unity
```

## Exposing types

When some value in the API "exposes a type", you are able to access the constructor (if available) and static members on the type via that value. For example, `unity.time` exposes Unity's `UnityEngine.Time` class, which has a static field called `deltaTime`, telling you the time elapsed since the last frame. To access it from Lua:

```
local deltaTime = unity.time.deltaTime
```

Note that Moonsharp, TECHMANIA's Lua interpreter, will convert `ref` and `out` parameters in C# methods to additional return values. For another example, `net.int` exposes .NET's `System.Int32` class, which has a static method `bool TryParse(string s, out int result)`. It tries to parse the input string into an integer, and writes the result to the `result` parameter if successful, and also returns a boolean indicating whether the conversion is successful. To access it from Lua:

```
local convertSuccess, result = net.int.TryParse("123")
```

To call the constructor of a type and create an instance, call the `__new` metamethod. For example:

```
local vector = unity.vector2.__new(123, 456)
```

Also note that there are some types that are not exposed as a value in the API table, but still available in the Lua environment, in that you can access instance members on values of that type.

## Exposing enums

When some value in the API "exposes an enum", treat each value in the enum as a constant value. For example, `tm.enum.ruleset` exposes the TECHMANIA `Options.Ruleset` enum with values `Standard`, `Legacy` and `Custom`. You can use it like:

```
local ruleset = tm.enum.ruleset.Standard
if (ruleset == tm.enum.ruleset.Legacy) then ... end
```

## `tm` object

The `tm` object gives you access to various TECHMANIA types. Refer to class [`ThemeApi.Techmania`](#class-themeapitechmania) for reference on members of this object, except `enum`.

## `tm.enum` table

`tm.enum` exposes the following TECHMANIA enums. Refer to the ["TECHMANIA enums"](#techmania-enums) section for reference on these enums.

|Key|Value|
|--|--|
|`eventType`|`ThemeApi.VisualElementWrap.EventType`|
|`ruleset`|`Options.Ruleset`|
|`gameState`|`GameState.State`|
|`skinType`|`SkinType`|
|Enums in tracks, patterns and notes|
|`controlScheme`|`ControlScheme`|
|`noteType`|`NoteType`|
|`curveType`|`CurveType`|
|Modifiers|
|`noteOpacity`|`Modifiers.NoteOpacity`|
|`scanlineOpacity`|`Modifiers.ScanlineOpacity`|
|`scanDirection`|`Modifiers.ScanDirection`|
|`notePosition`|`Modifiers.NotePosition`|
|`scanPosition`|`Modifiers.ScanPosition`|
|`fever`|`Modifiers.Fever`|
|`keysound`|`Modifiers.Keysound`|
|`assistTick`|`Modifiers.AssistTick`|
|`suddenDeath`|`Modifiers.SuddenDeath`|
|`mode`|`Modifiers.Mode`|
|`controlOverride`|`Modifiers.ControlOverride`|
|`scrollSpeed`|`Modifiers.ScrollSpeed`|
|Enums related to scores|
|`judgement`|`Judgement`|
|`performanceMedal`|`PerformanceMedal`|
|`feverState`|`ScoreKeeper.FeverState`|

## `net` table

The `net` table exposes the following .NET types. You can find documentation on these types by searching in the [.NET API browser](https://learn.microsoft.com/en-us/dotnet/api/?view=netframework-4.0), or by searching ".NET \<type name\>" in a search engine. Note that the TECHMANIA project uses .NET Framework 4.0.

|Key|Value|
|--|--|
|`bool`|`System.Boolean` (aka. `bool`)|
|`int`|`System.Int32` (aka. `int`)|
|`float`|`System.Single` (aka. `float`)|
|`string`|`ThemeApi.StringWrap`*|
|`dateTime`|`System.DateTime`|

\* Moonsharp converts .NET strings to Lua strings, so we are unable to call instance methods in `System.String` on strings. To work around this, we provide the `ThemeApi.StringWrap` class, making available most `System.String` instance methods as static methods. Refer to the reference on class `ThemeApi.StringWrap` for details and examples.

## `unity` table

The `unity` table exposes the following Unity types. You can find documentation on these types by searching in the [Unity Scripting Reference](https://docs.unity3d.com/2022.2/Documentation/ScriptReference/index.html), or by searching "Unity \<type name\>" in a search engine. Note that the TECHMANIA project uses Unity 2022.2.

|Key|Value|
|--|--|
|`time`|`UnityEngine.Time`|
|Types related to display|
|`screen`|`UnityEngine.Screen`|
|`resolution`|`UnityEngine.Resolution`|
|`refreshRate`|`UnityEngine.RefreshRate`|
|Types related to input|
|`input`|`UnityEngine.Input`|
|`touch`|`UnityEngine.Touch`|
|Types related to maths|
|`mathf`|`UnityEngine.Mathf`|
|`random`|`UnityEngine.Random`|
|`vector2`|`UnityEngine.Vector2`|
|`vector3`|`UnityEngine.Vector3`|
|Types related to mesh generation|
|`vertex`|`UnityEngine.UIElements.Vertex`|
|Types related to UXML styles|
|`styleBackground`|`UnityEngine.UIElements.StyleBackground`|
|`background`|`UnityEngine.UIElements.Background`|
|`styleColor`|`UnityEngine.UIElements.StyleColor`|
|`color`|`UnityEngine.Color`|
|`color32`|`UnityEngine.Color32`|
|`styleFloat`|`UnityEngine.UIElements.StyleFloat`|
|`styleInt`|`UnityEngine.UIElements.StyleInt`|
|`styleLength`|`UnityEngine.UIElements.StyleLength`|
|`length`|`UnityEngine.UIElements.Length`|
|`styleTranslate`|`UnityEngine.UIElements.StyleTranslate`|
|`translate`|`UnityEngine.UIElements.Translate`|
|`styleRotate`|`UnityEngine.UIElements.StyleRotate`|
|`rotate`|`UnityEngine.UIElements.Rotate`|
|`angle`|`UnityEngine.UIElements.Angle`|
|`styleScale`|`UnityEngine.UIElements.StyleScale`|
|`scale`|`UnityEngine.UIElements.Scale`|
|`styleTransformOrigin`|`UnityEngine.UIElements.StyleTransformOrigin`|
|`transformOrigin`|`UnityEngine.UIElements.TransformOrigin`|
|Types related to `Painter2D`|
|`painter2D`|`UnityEngine.UIElements.Painter2D`|
|`vectorImage`|`UnityEngine.UIElements.VectorImage`|
|`gradient`|`UnityEngine.Gradient`|
|`gradientColorKey`|`UnityEngine.GradientColorKey`|
|`gradientAlphaKey`|`UnityEngine.GradientAlphaKey`|

## `unity.enum` table

The `unity.enum` table exposes the following Unity enums.

|Key|Value|
|--|--|
|Enums related to display|
|`fullScreenMode`|`UnityEngine.FullScreenMode`|
|Enums related to input|
|`touchPhase`|`UnityEngine.TouchPhase`|
|Enums related to UXML styles|
|`styleKeyword`|`UnityEngine.UIElements.StyleKeyword`|
|`lengthUnit`|`UnityEngine.UIElements.LengthUnit`|
|`angleUnit`|`UnityEngine.UIElements.AngleUnit`|
|Enums related to `Painter2D`|
|`lineCap`|`UnityEngine.UIElements.LineCap`|
|`lineJoin`|`UnityEngine.UIElements.LineJoin`|
|`fillRule`|`UnityEngine.UIElements.FillRule`|
|`arcDirection`|`UnityEngine.UIElements.ArcDirection`|
|`colorSpace`|`UnityEngine.ColorSpace`|
|`gradientMode`|`UnityEngine.GradientMode`|
|Enums related to events|
|`propagationPhase`|`UnityEngine.UIElements.PropagationPhase`|
|`keyCode`|`UnityEngine.KeyCode`|

# TECHMANIA classes

Classes within this section are sorted alphabetically, but properties and methods within each class are grouped by usage. To quickly find references for a class, press Ctrl+F and search for "class \<classname\>".

## Class `AudioSourceManager`

Allows the playback of audio clips. You can access an `AudioSourceManager` object via `tm.audio`.

### Playing audio

```
UnityEngine.AudioSource PlayMusic(UnityEngine.AudioClip clip,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
UnityEngine.AudioSource PlayKeysound(UnityEngine.AudioClip clip,
    bool hiddenLane,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
UnityEngine.AudioSource PlaySfx(UnityEngine.AudioClip clip,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
```

Plays the specified audio clip on the music / keysound / SFX channel, and returns the `AudioSource` playing the clip. You can load an audio clip from the theme with `tm.io.LoadAudioFromTheme`, or from the disk with `tm.io.LoadAudioFromFile`.

The clip will start playing from `startTime` seconds. `volumePercent` is between 0 and 100. `panPercent` is between -100 (left) and 100 (right).

For keysounds, in addition to the clip you also need to specify whether the note is in a playable lane or hidden lane. Keysounds in hidden lanes will be played through the music channel.

Please note that there are software and hardware limits on how many sounds per channel can play simultaneously. The software limits are:

* 1 music clip
* 32 keysounds in playable lanes
* 64 keysounds in hidden lanes
* 8 SFX clips

If the limits are reached and either TECHMANIA or your theme plays another clip, it will cause one currently playing clip to stop.

### Controlling all audio sources

```
void PauseAll()
void UnpauseAll()
void StopAll()
```

Pauses, unpauses or stops all `AudioSource`s.

```
void SetSpeed(float speed)
```

Sets the playing speed of all `AudioSource`s. The normal speed is 1.

```
bool IsAnySourcePlaying()
```

Returns if there is currently any `AudioSource` playing audio.

### Miscellaneous

```
void ApplyVolume()
```

Retrieves the volume values from TECHMANIA options and applies them to the `AudioSource`s. You need to call this after changing the volume values in options for them to take effect.

## Class `BpmEvent`

Objects of this class denote BPM changes in a pattern.

```
int pulse
double bpm
```

At `pulse`, change BPM to `bpm`.

```
float time
```

`pulse` converted to time. Only available after calling `PrepareForTimeCalculation` on the pattern.

## Class `ComboSkin`

One of the 4 types of skins.

```
string author

float distanceToNote
float height
float spaceBetweenJudgementAndCombo

SpriteSheet feverMaxJudgement
SpriteSheet rainbowMaxJudgement
SpriteSheet maxJudgement
SpriteSheet coolJudgement
SpriteSheet goodJudgement
SpriteSheet missJudgement
SpriteSheet breakJudgement

List<SpriteSheet> feverMaxDigits
List<SpriteSheet> rainbowMaxDigits
List<SpriteSheet> maxDigits
List<SpriteSheet> coolDigits
List<SpriteSheet> goodDigits
```

Refer to [Skins](../Skins.md#combo-skin) for an explanation of these fields.

## Class `DragNode`

One node of a drag note, consisting of one anchor, and optionally left and right control points, if the drag note uses a Bezier curve.

```
FloatPoint anchor
FloatPoint controlLeft
FloatPoint controlRight
```

Anchor's position is relative to the note head, and control points' positions are relative to the anchor.

## Class `DragNote`

A derived class of [`Note`](#class-note), this class uniquely covers the note type of drag.

Inherited members:

```
NoteType type
int pulse
int lane
string sound
int volumePercent
int panPercent
bool endOfScan
int GetScanNumber(int bps)
float time
```

See [`Note`](#class-note).

```
CurveType curveType
```

The curve type. If using B-spline, control points in drag nodes are ignored.

```
List<DragNode> nodes
```

The nodes in this drag note. These determine the curve's shape. See [`DragNode`](#class-dragnode). There must be at least 2 nodes in each drag note, and the first node's anchor must be at (0, 0).

```
float gracePeriodLength
```

The grace period's length, in seconds. Only available after calling `CalculateTimeOfAllNotes` on the pattern.

```
float endTime
```

The time of the note's end. Only available after calling `CalculateTimeOfAllNotes` on the pattern.

## Class `EditorOptions`

Contains options specific to the editor. The editor has its own options screen for these options, so it's OK for you theme to ignore them.

```
int beatSnapDivisor
int visibleLanes
bool showKeysounds
bool keepScanlineInView
bool applyKeysoundToSelection
bool applyNoteTypeToSelection
bool lockNotesInTime
bool lockDragAnchorsInTime
bool snapDragAnchors
bool metronome
bool assistTickOnSilentNotes
bool returnScanlineAfterPlayback
```

You can find explanations in the editor options screen.

## Class `FloatPoint`

A named tuple of lane and pulse. Anchors and control points in drag nodes are all `FloatPoint` objects.

```
float lane
float pulse
```

## Class `GameUISkin`

One of the 4 types of skins.

Refer to [Skins](../Skins.md#game-ui-skin) for an explanation of these fields.

## Class `GlobalResource`

Holds resources that are always available from the moment your theme loads, hence "global". Access this type via `tm.resources`.

### Resources on skins

```
NoteSkin noteSkin
VfxSkin vfxSkin
ComboSkin comboSkin
GameUISkin gameUiSkin
```

The currently loaded skins.

```
List<string> GetSkinList(SkinType type)
```

Returns the names of all available skins of the specified type.

### Resources on tracks

```
List<TrackSubfolder> GetSubfolders(string parent)
List<TrackInFolder> GetTracksInFolder(string parent)
List<TrackWithError> GetTracksWithError(string parent)
```

Returns all [`TrackSubfolder`](#class-globalresourcetracksubfolder), [`TrackInFolder`](#class-globalresourcetrackinfolder) or [`TrackWithError`](#class-globalresourcetrackwitherror) objects in the specified parent folder, corresponding to, respectively, subfolders in it, tracks in it (but not in a subfolder), and tracks with errors (due to I/O errors, deserialization errors, etc.) in it.

`parent` should be the track root folder, available at `tm.paths.GetTrackRootFolder()`, or a subfolder of it.

```
void ClearTrackList()
```

Clears the in-memory track list to release memory. After calling this, `GetSubfolders`, `GetTracksInFolder` and `GetTracksWithError` will stop working.

```
bool anyOutdatedTrack
```

Whether there is any track in an outdated format on disk. Currently there is no way to query which specific track is outdated.

### Resource on themes

```
List<string> GetThemeList()
```

Returns the names of all available themes.

## Class `GlobalResource.TrackSubfolder`

Describes a subfolder in some parent folder given to `GlobalResource.GetSubfolders`.

```
string name
string fullPath
DateTime modifiedTime
```

The name, full path, and last modified time of this subfolder.

```
string eyecatchFullPath
```

The full path to the eyecatch image inside this subfolder, if any. If a subfolder does not have an eyecatch, the value is nil.

## Class `GlobalResource.TrackInFolder`

Describes a track in some parent folder given to `GlobalResource.GetTracksInFolder`.

```
string folder
```

The full path to the folder that holds the `track.tech` file. File names in the track, such as BGAs and keysounds, should be concatenated to this folder to get the full path.

```
DateTime modifiedTime
```

The last modified time of `folder`.

```
Track minimizedTrack
```

The minimized track loaded from `track.tech`. To save memory, TECHMANIA discards all notes, BPM events and time stops from tracks when building the track list. If you need the full track, call `tm.io.LoadFullTrack`.

## Class `GlobalResource.TrackWithError`

Describes a track with error in some parent folder given to `GlobalResource.GetTracksWithError`.

```
string type
```

The type of the error with this track. Possible values are "Load" (an error when loading the track) and "Upgrade" (an error when upgrading the track's format).

```
Status status
```

A [`Status`](#class-status) object holding the detailed error.

## Class `HoldNote`

A derived class of [`Note`](#class-note), this class covers the note types that require holding: hold, repeat head hold, repeat hold.

Inherited members:

```
NoteType type
int pulse
int lane
string sound
int volumePercent
int panPercent
bool endOfScan
int GetScanNumber(int bps)
float time
```

See [`Note`](#class-note).

```
int duration
```

The duration of the hold, in pulses.

```
float gracePeriodLength
```

The grace period's length, in seconds. Only available after calling `CalculateTimeOfAllNotes` on the pattern.

```
float endTime
```

The time of the note's end. Only available after calling `CalculateTimeOfAllNotes` on the pattern.

## Class `LegacyRulesetOverride`

Contains override values for the legacy ruleset, specific to the pattern that contains it.

```
List<float> timeWindows
List<int> hpDeltaBasic
List<int> hpDeltaChain
List<int> hpDeltaHold
List<int> hpDeltaDrag
List<int> hpDeltaRepeat
List<int> hpDeltaBasicDuringFever
List<int> hpDeltaChainDuringFever
List<int> hpDeltaHoldDuringFever
List<int> hpDeltaDragDuringFever
List<int> hpDeltaRepeatDuringFever
```

See [track.tech specification](../track.tech_specification.md). Empty lists mean no override.

```
bool HasAny()
```

A helper method to determine whether there is any override at all in this object.

## Class `Modifiers`

Contains the currently selected modifiers. Access the `Modifiers` object via `tm.options.modifiers`.

```
NoteOpacity noteOpacity
ScanlineOpacity scanlineOpacity
ScanDirection scanDirection
NotePosition notePosition
ScanPosition scanPosition
Fever fever
Keysound keysound
AssistTick assistTick
SuddenDeath suddenDeath
Mode mode
ControlOverride controlOverride
ScrollSpeed scrollSpeed
```

The current modifiers, which are all enums.

```
bool HasAnySpecialModifier()
```

A helper to determine whether any special modifier is selected.

## Class `Note`

The interactable elements in patterns. This base class covers the note types without a duration: basic, chain head, chain node, repeat head, repeat. The other types are represented by derived classes [`HoldNote`](#class-holdnote) and [`DragNote`](#class-dragnote).

```
NoteType type
int pulse
int lane
string sound
int volumePercent
int panPercent
bool endOfScan
```

The basic attributes of a note. A note is considered a hidden note if its lane (in 0-index) is smaller than the pattern's playable lanes.

`volumePercent` is in \[0, 100\]; `panPercent` is in \[-100, 100\].

```
int GetScanNumber(int bps)
```

A helper method to calculate the scan number (0-index) this note is in. This takes end-of-scan into consideration.

```
float time
```

The correct time of this note,. in seconds after the backing track begins. For hold and drag notes, this is for the start of the note. Only available after calling `CalculateTimeOfAllNotes` on the pattern.

## Class `NoteSkin`

One of the 4 types of skins.

Refer to [Skins](../Skins.md#note-skin) for an explanation of these fields.

## Class `Options`

Contains TECHMANIA options, per-track options and per-theme options. TECHMANIA will load options on startup, but it is up to the theme to save options if there are any changes. Access the current options via `tm.options`.

Also note that, many options will not take effect automatically after you modify their value. Read the description of these fields for what to call to apply the options.

```
string kVersion
```

The version of the serialization format. The current value is "3". Refer to [format version history](../Format_version_history.md) for more details.

```
void SaveToFile()
```

Saves the current options to disk.

### Graphics

```
int width
int height
int refreshRateNumerator
int refreshRateDenominator
UnityEngine.FullScreenMode fullScreenMode
bool vSync
```

Resolution, refresh rate, full screen mode and VSync. Call `ApplyGraphicsSettings()` to apply these. The refresh rate is expressed as a fraction to support non-integer refresh rates.

A list of all resolutions supported by the device is available at `unity.screen.resolutions`, which is a list of `UnityEngine.Resolution` structs.

```
void ApplyGraphicSettings()
```

Applies current graphic settings.

```
UnityEngine.Resolution GetCurrentResolutionAsObject()
```

Provides the current resolution as a `Resolution` struct.

```
void TemporarilyDisableVSync()
```

Temporarily disables VSync, regardless of whether it was true or false before the call. Useful for loading screens, as TECHMANIA can load no more than one thing per frame, so if the user's device can load things faster than one frame, VSync will cause unnecessary waiting.

You do not need to call `ApplyGraphicSettings()` after this.

```
void RestoreVSync()
```

Restores `vSync` to its previous value before calling `TemporarilyDisableVSync()`, whether it was true or false.

You do not need to call `ApplyGraphicSettings()` after this.

### Audio

```
int masterVolumePercent
int musicVolumePercent
int keysoundVolumePercent
int sfxVolumePercent
```

The volume of each audio channel, from 0 to 100. Call `ApplyVolumeSettings()` to apply these.

```
int audioBufferSize
```

The audio buffer size in samples. Usually a power of 2; default is 512. As the game explains, smaller value may reduce audio latency, but puts a higher strain on the user's system, and has a higher chance to cause audio stuttering or audio to stop altogether. Call `ApplyAudioBufferSize()` to apply this.

```
void ApplyVolumeSettings()
void ApplyAudioBufferSize()
```

Applies current volume / audio buffer size settings.

```
int GetDefaultAudioBufferSize()
```

Returns the default audio buffer size on the user's system.

### Appearance

```
string locale
```

The current locale. Refer to [`ThemeApi.ThemeL10n`](#class-themeapithemel10n) for more details on localization. Call `tm.l10n.ApplyLocale()` to apply this.

Note that, even if your theme does not use `ThemeApi.ThemeL10n`, this setting still applies to things outside of your theme, such as the boot screen and the editor.

```
string noteSkin
string vfxSkin
string comboSkin
string gameUiSkin
```

The names of the currently selected skins. After changing skins, call `tm.io.ReloadNoteSkin`, `tm.io.ReloadVfxSkin`, `tm.io.ReloadComboSkin` and `tm.io.ReloadGameUiSkin` respectively to load and apply the new skins.

```
bool reloadSkinsWhenLoadingPattern
```

Whether TECHMANIA should reload currently selected skins when loading a pattern. Useful for skin development.

```
string theme
```

Name of the currently selected theme. TECHMANIA does not support applying a theme at runtime; the user must restart TECHMANIA for the new theme to take effect.

### Timing

```
int touchOffsetMs
int touchLatencyMs
int keyboardMouseOffsetMs
int keyboardMouseLatencyMs
```

The note offset / input latency of touchscreen / keyboard and mouse, all in milliseconds. See [Offset and latency](../Offset_and_latency.md) for an explanation.

### Custom data location

```
bool customDataLocation
```

Whether to use custom data locations instead of the default ones. Call `tm.paths.ApplyCustomDataLocation()` to apply this.

```
string tracksFolderLocation
string skinsFolderLocation
string themesFolderLocation
```

The full paths of the tracks / skins / themes folder, if `customDataLocation` is true. Call `tm.paths.ApplyCustomDataLocation()` to apply these.

### Discord rich presence

```
bool discordRichPresence
```

Read only. Whether Discord rich presence is on.

```
void TurnOnDiscordRichPresence()
void TurnOffDiscordRichPresence()
```

Turns on / off Discord rich presence. These will take effect immediately.

### Miscellaneous

```
Ruleset ruleset
```

The currently chosen ruleset, an enum between standard, legacy and custom.

This (`tm.options.ruleset`) is different from the `Ruleset` object accessed at `tm.ruleset`, which contains the specific numbers in the currently loaded ruleset.

```
EditorOptions editorOptions
Modifiers modifiers
```

See [`EditorOptions`](#class-editoroptions) and [`Modifiers`](#class-modifiers).

### Per-track options

```
PerTrackOptions GetPerTrackOptions(string guid)
```

Returns the [`PerTrackOptions`](#class-pertrackoptions) object corresponding to the track identified by the guid. The guid of a track is available at its `trackMetadata.guid`. If no [`PerTrackOptions`](#class-pertrackoptions) object exists for the track, a new one will be created.

### Per-theme options

```
Dictionary<string, string> GetThemeOptions(string themeName)
```

Returns a `Dictionary` holding the theme options for the specified theme. You can read and write the dictionary as you see fit, and all content will be saved to disk with `SaveToFile()`.

From the user's perspective, a theme's name is its filename, which the user can change. To prevent themes from being disconnected from their options when a rename happens, we allow themes to identify themselves consistently with the `themeName` parameter.

All `Dictionary` methods are available on the returned object, but keep in mind that all values must be strings. You can use `Parse` methods on .NET types to convert strings to other types.

## Class `Paths`

Contains methods to retrieve paths of TECHMANIA-related folders, as well as methods to manipulate paths.

```
const string kTrackFilename = "track.tech"
```

The `track.tech` filename as a constant.

### Resource folders

There is a set of track, skin and theme folders in an on-disk location (either next to TECHMANIA.exe or in custom data locations), and another set in "streaming assets". See [Folders and zips](../Folders_and_zips.md) for an explanation of streaming assets.

In most cases, TECHMANIA will conceptually merge contents in streaming assets into the on-disk location, so you do not need to care about streaming assets at all. If for any reason you need a path specifically in streaming assets, you can pass `streamingAssets = true` to the corresponding method.

```
void ApplyCustomDataLocation()
```

Call this after changing any option related to custom data locations to apply them.

```
string GetTrackRootFolder(bool streamingAssets = false)
```

Returns the root folder for tracks. The track list in `GlobalResources` is built from this folder and its subfolders.

```
string GetSkinRootFolder(bool streamingAssets = false)
```

Returns the root folder for skins, which is the folder above the folders for skin types.

```
string GetSkinRootFolderForType(SkinType type, bool streamingAssets = false)
```

Returns the root folder for the specified type of skin, which is a subfolder of `GetSkinRootFolder`. The skin list of each type in `GlobalResources` is built from subfolders of this folder.

```
string GetSkinFolder(SkinType type, string name)
```

Returns the folder containing the skin with the specified name.

```
string GetThemeFolder(bool streamingAssets = false)
```

Returns the folder for themes. The theme list in `GlobalResources` is built from files in this folder.

```
string GetThemeFilePath(string name)
```

Returns the path to the theme file with the specified name.

```
string GetOptionsFilePath()
string GetRulesetFilePath()
string GetRecordsFilePath()
```

Returns the path to the options file, ruleset file and records file, respectively.

### Path manipulation

```
string RemoveCharsNotAllowedOnFileSystem(string input)
```

Returns a copy of `input` with the following characters removed: `\/*:?\"<>|`

```
string FullPathToUri(string fullPath)
```

Converts the path to a URI in the `file://` scheme.

```
string HidePlatformInternalPath(string fullPath)
```

On mobile, this removes parts of `fullPath` that expose the OS's internal file system. The output path is no longer a valid path, and only suitable for display.

On PC, this returns `fullPath` itself.

```
string RelativePath(string reference, string absolutePath)
```

Converts `absolutePath` to a relative path in reference to `reference`.

```
string EscapeBackslash(string path)
```

Replaces `\` with `\\`, so that paths on Windows do not form escape sequences when displayed on a visual element.

```
string GoUpFrom(string path)
```

Returns `path`'s parent folder. However, this does not go higher than `GetTrackRootFolder`; the return value for that folder is itself.

```
string Combine(string path1, string path2)
```

Combines the path parts.

```
bool IsInStreamingAssets(string path)
```

Determines whether the input path is in streaming assets.

## Class `Pattern`

The unit of gameplay. A track contains any number of patterns.

### Pattern content

```
PatternMetadata patternMetadata
LegacyRulesetOverride legacyRulesetOverride
```

See [`PatternMetadata`](#class-patternmetadata) and [`LegacyRulesetOverride`](#class-legacyrulesetoverride).

```
List<BpmEvent> bpmEvents
List<TimeStop> timeStops
```

The list of BPM events and time stops in the pattern. See [`BpmEvent`](#class-bpmevent) and [`TimeStop`](#class-timestop).

```
List<Note> NotesAsList()
```

Provides the list of notes in the pattern. This method converts notes from an internal data structure to a list when called, so it can be costly. Do not call this frequently. See [`Note`](#class-note).

```
const int pulsesPerBeat = 240
```

Useful for converting between pulses, beats and scans. The number of beats per scan is available in pattern metadata.

```
bool ShouldPlayInMusicChannel(int lane)
```

A helper method to determine whether the keysound of a note in the specified lane should be played in the music channel or keysound channel.

```
bool IsHidden(int lane)
```

A helper method to determine whether the specified lane is a hidden lane.

### Timing calculation

```
void PrepareForTimeCalculation()
```

Calculates and fills the `time`, `endTime` and `bpmAtStart` fields of all BPM events and time stops in this patterns. This enables other timing-related methods on the pattern.

```
void GetLengthInSecondsAndScans(out float seconds, out int scans)
```

Calculates and returns the pattern's length in both seconds and scans. Requires that `PrepareForTimeCalculation()` has been called. In Lua, this method takes no parameters and returns 2 values: `local seconds, scans = pattern.GetLengthInSecondsAndScans()`.

```
void CalculateTimeOfAllNotes()
```

Calculates and fills the `time`, `endTime` and `gracePeriodLength` fields of all notes in this pattern. Requires that `PrepareForTimeCalculation()` has been called.

```
float TimeToPulse(float time)
float PulseToTime(float pulse)
```

Converts between time and pulse. Requires that `PrepareForTimeCalculation()` has been called.

```
float GetBPMAt(float pulse)
```

Calculates and returns the BPM at the specified pulse. Requires that `PrepareForTimeCalculation()` has been called.

### Statistics and radar

```
int NumPlayableNotes()
```

Calculates and returns the number of playable notes in the pattern.

```
Pattern.Radar CalculateRadar()
```

Calculates and returns the [`Radar`](#struct-patternradar) of this pattern. Requires that `PrepareForTimeCalculation()` has been called.

### Fingerprint

```
void CalculateFingerprint()
```

Calculates the fingerprint of this pattern.

```
string fingerprint
```

Read-only. The pattern's fingerprint. Only available after calling `CalculateFingerprint()`.

## Struct Pattern.RadarDimension

Describes one dimension of a radar.

```
float raw
int normalized
```

The raw and normalized values of this dimension. `normalized` is in \[0, 100\]. See [Radar](../Radar.md).

## Struct Pattern.Radar

A struct to describe a pattern from various dimensions. Calculate the radar of a pattern with `Pattern.CalculateRadar`. See [Radar](../Radar.md).

```
Pattern.RadarDimension density
Pattern.RadarDimension peak
Pattern.RadarDimension speed
Pattern.RadarDimension chaos
Pattern.RadarDimension async
```

The 5 main dimensions.

```
float suggestedLevel
int suggestedLevelRounded
```

The suggested level of the pattern, calculated from the radar dimensions.

## Class `PatternMetadata`

```
string guid
string patternName
int level
ControlScheme controlScheme
int playableLanes
string author
string backingTrack
string backImage
string bga
double bgaOffset
bool waitForEndOfBga
bool playBgaOnLoop
double firstBeatOffset
double initBpm
int bps
```

See [track.tech specification](../track.tech_specification.md).

## Class `PerTrackOptions`

Contains options specific to a track. Access the `PerTrackOptions` for a track via `Options.GetPerTrackOptions`.

```
bool noVideo
```

Whether to hide the track's BGA, if any. If on, the background image will display in the BGA's place.

```
int backgroundBrightness
```

In \[0, 10\], where 10 is full brightness. Applies to both BGA and background image.

## Class `Record`

Stores the record for a specific pattern under a specific ruleset. Retrieve records with `tm.records.GetRecord`.

```
string guid
string fingerprint
Options.Ruleset ruleset
int score
PerformanceMedal medal
string gameVersion
```

The fields in the record. `guid` and `fingerprint` are of the pattern. All fields are read only.

```
string Rank()
```

Converts `score` to a letter rank. See [`ScoreKeeper`](#class-scorekeeper)``.ScoreToRankAssumingStageClear``.

## Class `Records`

Holds all of the player's records. Access the instance via `tm.records`.

```
string kVersion
```

The version of the serialization format. The current value is "1". Refer to [format version history](../Format_version_history.md) for more details.

```
Record GetRecord(Pattern p, Options.Ruleset ruleset)
Record GetRecord(Pattern p)
```

Retrieves the [`Record`](#class-record) for the specified [`Pattern`](#class-pattern) and ruleset. The entire pattern must be passed in because as part of the process, TECHMANIA will calculate the input pattern's fingerprint, and only return a record if the fingerprint matches.

The `ruleset` is the `Options.Ruleset` enum, not a `Ruleset` object. It is also optional. If omitted, this method will retrieve record for the currently selected ruleset. Note that no record will ever be created for the custom ruleset.

If no record exists for the specified pattern and ruleset, returns nil.

## Class `Ruleset`

Stores the actual numbers in a ruleset. Not to be confused with `Options.ruleset`, whose value is an enum `Ruleset`. Access the numbers in the currently selected ruleset via `tm.ruleset`.

```
string kVersion
```

The version of the serialization format. The current value is "2". Refer to [format version history](../Format_version_history.md) for more details.

```
List<float> timeWindows
bool timeWindowsInPulses
float longNoteGracePeriod
bool longNoteGracePeriodInPulses
List<float> scanMarginTopBottom
List<float> scanMarginMiddle
float scanMarginBeforeFirstBeat
float scanMarginAfterLastBeat
float hitboxWidth
float hitboxHeight
float chainHeadHitboxWidth
float chainNodeHitboxWidth
float dragHitboxWidth
float dragHitboxHeight
float ongoingDragHitboxWidth
float ongoingDragHitboxHeight
int maxHp
List<int> hpDeltaBasic
List<int> hpDeltaChain
List<int> hpDeltaHold
List<int> hpDeltaDrag
List<int> hpDeltaRepeat
List<int> hpDeltaBasicDuringFever
List<int> hpDeltaChainDuringFever
List<int> hpDeltaHoldDuringFever
List<int> hpDeltaDragDuringFever
List<int> hpDeltaRepeatDuringFever
bool comboBonus
bool constantFeverCoefficient
int feverBonusOnMax
int feverBonusOnCool
int feverBonusOnGood
```

See [Rulesets](../Rulesets.md).

```
Ruleset standard
Ruleset legacy
Ruleset custom
```

The 3 available rulesets.

```
Status LoadCustomRuleset()
```

Attempts to load the custom ruleset from disk, and returns a [`Status`](#class-status) indicating whether it succeeded. If the custom ruleset is loaded, it is accessible from the `custom` field.

Note that the game will call this internally when loading a pattern, if the custom ruleset is selected.

## Class `ScoreKeeper`

Keeps track of score, combo, HP and fever. Access the `ScoreKeeper` instance via `tm.game.scoreKeeper`. This instance is available in states `GameState.State.LoadComplete`, `GameState.State.Ongoing`, `GameState.State.Paused` and `GameState.State.Complete`.

```
bool stageFailed
```

Whether the player has failed to clear the stage due to losing all HP. If true, the score will be invalid.

### Score

```
int totalNotes
```

Returns the total number of playable notes in the pattern.

```
int NumNotesWithJudgement(Judgement judgement)
```

Returns the current number of notes resolved with the specified judgement.

```
bool AllNotesResolved()
```

Returns whether all notes have been resolved.

```
int ScoreFromNotes()
int totalFeverBonus
int ComboBonus()
```

Returns the 3 parts that make up the total score: current score from notes, current cumulative fever bonus, and current combo bonus, respectively.

```
int TotalScore()
```

A helper that returns the sum of `ScoreFromNotes()`, `totalFeverBonus` and `ComboBonus()`.

```
int maxScore
```

The maximum possible score from notes on the current ruleset. If the ruleset enables combo bonus, this is 290,000. If the ruleset disables combo bonus, this is 300,000.

```
PerformanceMedal Medal()
```

Returns the best medal that the current score qualifies for. If the game is not complete yet, this assumes unresolved notes will be resolved with rainbow MAX.

```
string Rank()
```

Returns the rank of the current score, one of "S++", "S+", "S", "A++", "A+", "A", "B", "C", and "F" (on stage failed).

```
string ScoreToRankAssumingStageClear(int score)
```

A static method that returns the rank of the specified total score, instead of the current score. It also assumes no stage failed.

### Combo

```
int currentCombo
int maxCombo
```

The current combo and max combo so far.

### HP

```
int maxHp
int hp
```

The max HP and current HP, respectively.

### Fever

```
ScoreKeeper.FeverState feverState
```

The current fever state.

```
float feverAmount
```

The current fever amount, in \[0, 1\].

## Class `SpriteSheet`

Elements of skins. Each `SpriteSheet` object describes one image file and how it is sliced into sprites.

```
string filename
int rows
int columns
int firstIndex
int lastIndex
int padding
bool bilinearFilter
float scale
float speed
bool additiveShader
```

See [Skins](../Skins.md).

```
Texture2D texture
```

The texture in `filename` loaded into memory.

## Class `Status`

Contains either OK or an error message. Many I/O operations that may succedd or fail report the result with a `Status` object.

```
string code
```

The status code, one of the following:
* "OK": no error, operation successfully finished.
* "NotFound": some file is not found.
* "IOError": an I/O error when reading or writing a file.
* "FormatError": the format of some file is invalid.
* "OtherError": an error that does not fit any other category.

```
bool Ok()
```

A shortcut to check whether the code is "OK".

```
string errorMessage
string filePath
```

If the code is not "OK", these fields may contain a more detailed error message and/or the path of the file that caused the error.

## Class `ThemeApi.CalibrationPreview`

This interface allows your theme to render and control a preview of the offset and latency settings in `Options`. Access an instance via `tm.calibrationPreview`.

### Setup

```
ThemeApi.VisualElementWrap previewContainer
```

The `VisualElementWrap` to render the preview in. TECHMANIA will spawn elements as its children.

```
List<string> timingDisplayClasses
```

A list of USS classes that TECHMANIA will apply to the timing display texts under each note.

```
int timingDisplayMaxLines
```

The max number of lines to display in each timing display. Each line corresponds to 1 touch / keystroke / click on the corresponding note.

```
string earlyString
string lateString
```

The "early" and "late" strings. TECHMANIA will append them to each line in the timing display, in the format of "\<device\> \<time difference\> \<early/late string\>". This allows the timing display to be localized.

```
bool setEarlyLateColors
Color earlyColor
Color lateColor
```

Whether to set special colors for `earlyString` and `lateString`. Does not affect the rest of timing display. If set, these colors will override any styles in the USS classes.

### Controls

```
void Begin()
```

Begins the calibration preview. TECHMANIA will render 5 notes in `previewContainer`, a timing display text under each note, and a scanline that scans from left to right. TECHMANIA will also play a predefined music track in the background, and predefined keysounds on each note.

By default, the timing calculations will use note offset and input latency for touchscreen; call `SwitchToTouch()` and `SwitchToKeyboardMouse()` to change that.

```
void ResetSize()
```

Resets the layout of internal elements. Call this after changing the layout of `previewContainer`.

```
void SwitchToTouch()
void SwitchToKeyboardMouse()
```

Tells the calibration preview to use note offset and input latency for touchscreen / keyboard and mouse, respectively.

```
void Conclude()
```

Stops the preview and removes all elements in `previewContainer`.

## Class `ThemeApi.EditorInterface`

## Class `ThemeApi.GameSetup`

## Class `ThemeApi.GameState`

## Class `ThemeApi.IO`

## Class `ThemeApi.SkinPreview`

This interface allows your theme to render and control a preview of the skin settings in `Options`. Access an instance via `tm.skinPreview`.

### Setup

```
ThemeApi.VisualElementWrap previewContainer
```

The `VisualElementWrap` to render the preview in. TECHMANIA will spawn elements as its children.

```
float bpm
int lanes
```

The BPM and number of lanes in the preview.

```
Judgement judgement
int combo
bool fever
```

Options on the VFX and combo text rendered in the preview: which judgement to show, the combo number, and whether to show fever MAX instead of rainbow MAX / MAX.

### Controls

```
void Begin()
```

Begins the skin preview. TECHMANIA will render 1 note in `previewContainer`, and a scanline that scans from left to right.

No matter the number of playable lanes, the note will be rendered in the 2nd one from the top.

```
void ResetSize()
```

Resets the layout of internal elements. Call this after changing the layout of `previewContainer`.

```
void Conclude()
```

Stops the preview and removes all elements in `previewContainer`.

## Class `ThemeApi.StringWrap`

## Class `ThemeApi.Techmania`

The entry point to TECHMANIA's data and types. Also contains miscellaneous methods that don't really fit into another type.

### Accessing the visual tree

```
ThemeApi.VisualElementWrap root
```

Provides the root element in the visual tree. From here you can query other elements in the tree with the `Q` method, such as `tm.root.Q("element-name")`.

Note that, when TECHMANIA transitions to and away from your theme (such as to the editor), it does so by setting the `display` property of the root element. It is adviced that you never change this property on the root element from your theme, in order to not interfere with this transition.

```
UnityEngine.UIElements.PanelSettings panelSettings
```

Read only. Provides access to the panel settings of the UI Document.

```
void SetPanelSettings(path)
```

Sets the panel settings of the UI document. `path` is the path towards the `.asset` file, starting with `Assets/UI`. The default theme uses this method to customize the style of dropdowns.

### Accessing other TECHMANIA types

```
Options options
Ruleset ruleset
Records records
ThemeApi.ThemeL10n l10n
ThemeApi.GameSetup gameSetup
ThemeApi.GameState game
ThemeApi.EditorInterface editor
ThemeApi.SkinPreview skinPreview
ThemeApi.calibrationPreview calibrationPreview
AudioSourceManager audio
```

Provides access to the instances of the respective classes.

```
Paths paths
GlobalResource resources
ThemeApi.IO io
```

Exposes the respective classes.

### System dialogs

```
string[] OpenSelectFileDialog(string title,
    string currentDirectory,
    bool multiSelect,
    string[] supportedExtensionsWithoutDot)
```

Opens the operating system's "open file" dialog and blocks. When the user closes the dialog, returns the full path of the selected files, if any. If the user cancels the dialog, returns an empty array. Only confirmed to work on Windows.

`title` is the title of the dialog. `currentDirectory` is the directory that the dialog starts at; pass in "" to start from the working directory. `multiSelect` specifies whether the user can select multiple files. `supportedExtensionsWithoutDot` is an array of supported file extensions, all without the dot.

```
string OpenSelectFolderDialog(string title, string currentDirectory)
```

Opens the operating system's "open folder" dialog and blocks. When the user closes the dialog, returns the full path of the selected folder. If the user cancels the dialog, returns `nil`. Only confirmed to work on Windows.

`title` is the title of the dialog. `currentDirectory` is the directory that the dialog starts at; pass in "" to start from the working directory.

### Script execution

```
void ExecuteScriptFromTheme(string path)
```

Executes the Lua script in the specified file. `path` is the path towards the `.txt` file, starting with `Assets/UI`. This is the recommended way to split your script across multiple files, and execute them from the main script.

```
void ExecuteScript(string script)
```

Executes the input string as Lua script. While functionally the same as `ExecuteScriptFromTheme`, there is less debugger support for scripts executed this way, because MoonSharp does not know where the source file is.

```
int StartCoroutine(function function)
```

Executes the Lua function as a coroutine and returns its coroutine ID, which can be later passed to `StopCoroutine` to stop execution. Within a coroutine, you can call `coroutine.yield()` to pause execution, and TECHMANIA will automatically resume execution on the next frame.

TECHMANIA never reuses coroutine IDs, so you can assume each `StartCoroutine` call will return a unique ID.

```
bool IsCoroutineRunning(int id)
```

Queries whether the specified coroutine is currently running.

```
void StopCoroutine(int id)
```

Stops the specified coroutine if it's current running. If the specified coroutine is not running, this method does nothing.

### Miscellaneous

```
void HideVfxAndComboText()
void RestoreVfxAndComboText()
```

Hides / restores all VFX and combo text. Useful if you don't want these to obscure visual elements in your theme.

```
void SetDiscordActivity(string details, string state,
    bool showElapsedTime = false)
```

Updates the activity currently displayed in Discord Rich Presence. If the user is not running Discord, or Discord Rich Presence is not supported, or Discord Rich Presence is turned off by the user, this method does nothing.

`details` and `state` are two lines of text displayed in the activity. If both are set, `state` is displayed below `details`.

If you set `showElapsedTime = true`, the activity will display elapsed time that counts up automatically. Making consecutive calls with `showElapsedTime = true` will NOT reset the timer, but making a call with `showElapsedTime = false` will reset the timer.

```
void OpenURL(string url)
```

Opens the specified URL with the default browser. If the URL is a file or directory on the user's disk, this will open the File Explorer instead. Only confirmed to work on Windows.

```
string GetPlatform()
```

Returns the current platform, one of `"Windows"`, `"Linux"`, `"macOS"`, `"Android"`, `"iOS"` and `"Unknown"`.

```
string Version()
```

Returns TECHMANIA's version.

```
void Quit()
```

Quits TECHMANIA. If running in the Unity editor, this will quit the play mode.

```
Table enum
```

A Lua table exposing various TECHMANIA enums. Refer to [`tm.enum` table](#tmenum-table) for reference.

## Class `ThemeApi.ThemeL10n`

## Class `ThemeApi.UQueryStateWrap`

A `ThemeApi.UQueryStateWrap` object is created when calling `Query` on [`ThemeApi.VisualElementWrap`](#class-themeapivisualelementwrap).

```
void ForEach(function f)
```

Calls the specified function on each element that matches the query.

Example:

```
tm.root.Q("container").Query("child").ForEach(function(element)
    print("found one child: " .. element.name)
end)
```

## Class `ThemeApi.VideoElement`

## Class `ThemeApi.VisualElementWrap`

A wrapper around Unity's `UnityEngine.UIElements.VisualElement` class, providing various additional functionalities. Each object corresponds to one element in the visual tree.

Note that it is possible to create different `VisualElementWrap` objects on the same element, therefore when testing for equality, call `Equals` instead of comparing the `VisualElementWrap` objects themselves.

```
UnityEngine.UIElements.VisualElement inner
```

Provides the internal `VisualElement` object that the `VisualElementWrap` object wraps around.

```
bool Equals(UnityEngine.UIElements.VisualElement other)
```

Returns whether this element and `other` correspond to the same element.

### Properties

```
bool enabledInHierarchy
bool enabledSelf
```

Read only. Returns whether the element is enabled in the hierarchy / by itself, respectively. If an element itself is enabled, but one of its ancestors are disabled, the element is considered disabled in the hierarchy.

When an element is disabled in hierarchy, the `:disabled` USS pseudo-class is applied, and the element does not respond to clicks.

```
void SetEnabled(bool enabled)
```

Enables or disables the element.

```
string name
```

Name of the element.

```
bool pickable
```

Whether the element receives click events.

```
UnityEngine.UIElements.IResolvedStyle resolvedStyle
UnityEngine.UIElements.IStyle style
```

Read only. `style` gives the style on the element as specified in in-line styles or USS; `resolvedStyle` gives the actual, resolved style on the element at runtime.

While you can't change `style` to point to a different object, you can modify the object that it points to, in order to change the style of the element. Example: `tm.root.Q("test").style.left = unity.styleLength.__new(50)`

```
UnityEngine.UIElements.ITransform transform
```

Read only. Provides the element's transform, ie. translation, rotation and scale.

```
UnityEngine.Rect contentRect
UnityEngine.Rect localBound
UnityEngine.Rect worldBound
```

Read only.

`contentRect` is the rectangle within the element that holds its content, in the element's local space. It ignores all transformation. Border and padding shrink this rectangle.

`worldBound` is the rectangle that the element takes up in the world space, after all transformation and layout. Margin shifts this rectangle, while border and padding do not affect it.

`localBound` is like `worldBound`, but before applying the transformations and layouts of the element's ancestors in the visual tree.

### Type-specific properties

```
string text
```

For elements that hold text (`TextElement` and `Button`), this is that text.

```
float lowValue
float highValue
```

For elements that allow setting a numeric value between two ends (`Slider`, `SliderInt`, `Scroller`), these are the minimal value and maximum value.

```
float value
```

For elements that allow setting a numeric value (`Slider`, `SliderInt`, `Scroller`, `IntegerField`, `FloatField`), this is the current value. Setting it will trigger a change event.

```
string stringValue
```

For elements that allow setting a string value (`DropdownField`, `TextField`), this is the current value. Setting it will trigger a change event.

```
string boolValue
```

For elements that allow setting a boolean value (`Toggle`), this is the current value. Setting it will trigger a change event.

```
ThemeApi.VisualElementWrap horizontalScroller
ThemeApi.VisualElementWrap verticalScroller
```

For `ScrollView`, these give the horizontal and vertical `Scroller` elements.

```
List<string> choices
int index
```

For `DropdownField`, `choices` is the list of available choices in the dropdown, and `index` is the index of the currently chosen item.

```
void SetValueWithoutNotify(string newValue)
void SetValueWithoutNotify(float newValue)
void SetValueWithoutNotify(bool newValue)
```

Allows setting numeric, string or boolean value without triggering change events.

```
void ScrollTo(ThemeApi.VisualElementWrap child)
```

For `ScrollView`, this makes the element scroll to the specified element. Note that this will not work if called on the same frame after any change to the `ScrollView` itself or elements within it; you will need to wait one frame for layout to update before calling.

### Event handling

```
void RegisterCallback(ThemeApi.VisualElementWrap.EventType eventType,
    function callback,
    any data)
```

Registers a callback function for the specified event type. The `data` parameter is optional, and can be of any Lua type. If `data` is set, it will be passed back to the callback when called.

You can register multiple handlers for each event type on each element. This is one of the additional features `VisualElementWrap` provides over `VisualElement`.

The callback will be called with 3 arguments:
* The `ThemeApi.VisualElementWrap` receiving the event
* The event object
* `data`

Example:

```
tm.root.Q("test").RegisterCallback(tm.enum.eventType.Click, function(element, event, data)
    print("Click event received by " .. element.name .. " with data " .. data)
end, "data")
```

```
void UnregisterCallback(ThemeApi.VisualElementWrap.EventType eventType,
    function callback)
```

Unregisters a callback function for the specified event type. If the callback has not been registered, this method will do nothing.

```
void UnregisterAllCallbackOfType(ThemeApi.VisualElementWrap.EventType eventType)
```

Unregisters all callback functions for the specified event type.

```
void UnregisterAllCallback()
```

Unregisters all callback functions for all event types.

### Querying

```
ThemeApi.VisualElementWrap Q(string name, string className = null)
```

Queries and returns the first element within the current element's children that matches the specified name (and optionally USS class name). If no child matches the criterias, returns `nil`.

```
ThemeApi.UQueryStateWrap Query(string name, string className = null)
```

Queries and returns a `UQueryStateWrap` object that allows traversing all child elements that match the specified name (and optionally USS class name). Refer to `UQueryStateWrap` for an example.

### USS classes

```
IEnumerable<string> GetClasses()
```

Returns a list of USS classes that this element has.

```
bool ClassListContains(string className)
```

Returns whether the element has the specified class.

```
void AddToClassList(string className)
void RemoveFromClassList(string className)
```

Adds / removes the specified class to / from the element.

```
void ClearClassList()
```

Removes all classes.

```
void EnableInClassList(string className, bool enable)
```

Adds or removes the specified class to or from the element, controlled by `enable`.

```
void ToggleInClassList(string className)
```

Toggles the specified class between on and off.

### Style shortcuts

```
bool display
bool visible
```

Whether the element is displayed / visible. Setting either to `false` will hide the element, but setting `display = false` will remove the element from layout consideration, while setting `visible = false` will still have the element take up its space in the layout.

Note that, if a `visible = false` element has a child with `visible = true`, the child will override its parent's `visible` and be visible. This does not apply to `display`.

```
UnityEngine.Texture2D backgroundImage
```

Allows getting or setting the background image as a `Texture2D`.

### Visual tree traversal

```
int childCount
```

Gives the number of direct children.

```
ThemeApi.VisualElementWrap parent
```

Gives the parent element.

```
IEnumerable<ThemeApi.VisualElementWrap> Children()
```

Returns a list of all direct children. Note that the returned array will be 1-indexed in Lua.

```
ThemeApi.VisualElementWrap InstantiateTemplate(string path)
```

Instantiates the specified UXML document under the current element. `path` is the path towards the `.uxml` file, starting with `Assets/UI`.

This is the only way to add non-empty elements to the visual tree at runtime.

```
ThemeApi.VisualElementWrap AddEmptyChild()
```

Adds and returns an empty child element.

```
void RemoveFromHierarchy()
void RemoveAllChildren()
```

`RemoveFromHierarchy` removes the current element and all its children from the visual tree; `RemoveAllChildren` removes all children, but not the current element. In either case, all event callbacks on the removed elements will be automatically unregistered.

```
void AddChild(ThemeApi.VisualElementWrap child)
void InsertChild(int index, ThemeApi.VisualElementWrap child)
```

Moves the specified element so it becomes a child of the current element. `AddChild` adds the element as the last child, while `InsertChild` inserts at the specified index.

```
void PlaceBehind(ThemeApi.VisualElementWrap sibling)
void PlaceInFront(ThemeApi.VisualElementWrap sibling)
```

Moves the current element so it is just before / after the specified sibling as child elements of their parent.

```
void BringToFront()
void SendToBack()
```

Moves the current element so it is the last / first child element of its parent.

### Custom mesh

```
void SetMeshGeneratorFunction(function function)
```

Sets a mesh generator function that will be called when Unity needs to repaint the element. The function takes 2 arguments:
* The `ThemeApi.VisualElementWrap` object to generate mesh for
* A `UnityEngine.UIElements.MeshGenerationContext` object that the function can use to draw custom mesh

When drawing custom mesh, (0, 0) is the top left, all coordinates are in pixels. `contentRect` gives the available area for drawing.

```
void MarkDirtyRepaint()
```

Forces a repaint of the element.

### Focus

```
void Focus()
```

Gives focus to this element. This may be useful for keyboard control, since keyboard events are sent to the element that has focus.

```
ThemeApi.VisualElementWrap FocusedElement()
```

Returns the element that currently has focus.

### Transform

Some terminology:
* Local space: the space defined by an element's `contentRect`. (0, 0) is top left; Y positive is downwards. You can place a child at a specific coordinate in an element's local space by setting `left` and `top` in the child's style.
* Screen space: the space defined by the screen resolution. (0, 0) is bottom left; Y positive is upwards. When you query from `UnityEngine.Input` the coordinates of the mouse cursor or a touch, they are in the screen space.

```
UnityEngine.Vector2 LocalSpaceToScreenSpace(UnityEngine.Vector2 localSpace)
```

Converts a point in the element's local space to screen space. 

```
UnityEngine.Vector2 ScreenSpaceToLocalSpace(UnityEngine.Vector2 screenSpace)
```

Converts a point in the screen space to the element's local space.

```
bool ContainsPointInScreenSpace(UnityEngine.Vector2 screenSpace)
```

Returns whether the element contains the specified point in screen space.

## Class `TimeStop`

Objects of this class denote time stops in a pattern.

```
int pulse
int duration
```

At `pulse`, stop time for `duration` beats.

```
float time
```

`pulse` converted to time. Only available after calling `PrepareForTimeCalculation` on the pattern.

```
float endTime
```

The time when the time stop ends. Only available after calling `PrepareForTimeCalculation` on the pattern.

```
double bpmAtStart
```

The BPM when the time stop begins. Only available after calling `PrepareForTimeCalculation` on the pattern.

## Class `Track`

A container of patterns corresponding to the same musical track. The contents of `track.tech` files are serialized `Track` objects.

```
string kVersion
```

The version of the serialization format. The current value is "3". Refer to [format version history](../Format_version_history.md) for more details. For tracks serialized in earlier format versions, TECHMANIA will upgrade them after deserialization, so when you read this field, it should always be "3".

```
TrackMetadata trackMetadata
```

See [`TrackMetadata`](#class-trackmetadata).

```
List<Pattern> patterns
```

The array of patterns inside the track. See [`Pattern`](#class-pattern).

## Class `TrackMetadata`

```
string guid
string title
string artist
string genre
string additionalCredits
string eyecatchImage
string previewTrack
double previewStartTime
double previewEndTime
string previewBga
bool autoOrderPatterns
```

See [track.tech specification](../track.tech_specification.md).

## Class `VfxSkin`

One of the 4 types of skins.

Refer to [Skins](../Skins.md#vfx-skin) for an explanation of these fields.

# TECHMANIA enums

## Enum `ControlScheme`

## Enum `CurveType`

## Enum `GameState.State`

## Enum `Judgement`

## Enum `Modifiers.AssistTick`

## Enum `Modifiers.ControlOverride`

## Enum `Modifiers.Fever`

## Enum `Modifiers.Keysound`

## Enum `Modifiers.Mode`

## Enum `Modifiers.NoteOpacity`

## Enum `Modifiers.NotePosition`

## Enum `Modifiers.ScanDirection`

## Enum `Modifiers.ScanlineOpacity`

## Enum `Modifiers.ScanPosition`

## Enum `Modifiers.ScrollSpeed`

## Enum `NoteType`

## Enum `Options.Ruleset`

## Enum `PerformanceMedal`

## Enum `ScoreKeeper.FeverState`

## Enum `VisualElementWrap.EventType`
