# TECHMANIA Theme API scripting reference

Applies to API version: 3

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
|`platform`|`ThemeApi.Techmania.Platform`|
|`statusCode`|`Status.Code`|
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
|Enums related to setlists|
|`hiddenPatternCriteriaType`|`Setlist.HiddenPatternCriteriaType`|
|`hiddenPatternCriteriaDirection`|`Setlist.HiddenPatternCriteriaDirection`|

## `net` table

The `net` table exposes the following .NET types. You can find documentation on these types by searching in the [.NET API browser](https://learn.microsoft.com/en-us/dotnet/api/?view=netframework-4.0), or by searching ".NET \<type name\>" in a search engine. Note that the TECHMANIA project uses .NET Framework 4.0.

|Key|Value|
|--|--|
|`bool`|`System.Boolean` (aka. `bool`)|
|`int`|`System.Int32` (aka. `int`)|
|`float`|`System.Single` (aka. `float`)|
|`string`|`ThemeApi.StringWrap`*|
|`dateTime`|`System.DateTime`|

\* Moonsharp converts .NET strings to Lua strings, so we are unable to call instance methods in `System.String` on strings. To work around this, we provide the `ThemeApi.StringWrap` class, making available most `System.String` instance methods as static methods. Refer to [`ThemeApi.StringWrap`](#class-themeapistringwrap) for details and examples.

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

## Class `AudioManager`

Allows the playback of sounds. You can access an `AudioManager` object via `tm.audio`.

Starting 2.1 (API version 2), TECHMANIA uses FMOD as its audio backend.
* When you load an audio file into memory, it is a "sound", in the form of a [`FmodSoundWrap`](#class-fmodsoundwrap) object. Some APIs may use the term "clip" for backwards compatibility.
* When you play a sound, it is played in a "channel", in the form of a [`FmodChannelWrap`](#class-fmodchannelwrap) object.
* Channels are routed into "channel group"s. TECHMANIA maintains four channel groups, for music, keysound, SFX, and master, respectively. You do not have control over channel groups other than volume.

### Playing audio

```
FmodChannelWrap PlayMusic(FmodSoundWrap sound,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
FmodChannelWrap PlayKeysound(FmodSoundWrap sound,
    bool hiddenLane,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
FmodChannelWrap PlaySfx(FmodSoundWrap sound,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
```

Plays the specified sound on the music / keysound / SFX channel group, and returns the `FmodChannelWrap` playing the sound. You can load a sound from the theme with `tm.io.LoadAudioFromTheme`, or from the disk with `tm.io.LoadAudioFromFile`.

The sound will start playing from `startTime` seconds. `volumePercent` is between 0 and 100. `panPercent` is between -100 (left) and 100 (right).

For keysounds, in addition to the sound you also need to specify whether the note is in a playable lane or hidden lane. Keysounds in hidden lanes will be played through the music channel group.

Please note that there are software and hardware limits on how many sounds can play simultaneously. If the limits are reached and either TECHMANIA or your theme plays another sound, it will cause one currently playing sound to stop.

### Controlling all channels

```
void PauseAll()
void UnpauseAll()
void StopAll()
```

Pauses, unpauses or stops all `FmodChannelWrap`s.

```
void SetSpeed(float speed)
```

Sets the playing speed of all `FmodChannelWrap`s. The normal speed is 1.

```
bool IsAnySoundPlaying()
```

Returns if there is currently any `FmodChannelWrap` playing audio.

```
bool IsAnySourcePlaying()
```

Deprecated. Alias of `IsAnySoundPlaying()`, kept for backwards compatibility.

### Miscellaneous

```
void ApplyVolume()
```

Retrieves the volume values from TECHMANIA options and applies them to the channel groups. You need to call this after changing the volume values in options for them to take effect.

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

## Class `FmodChannelWrap`

A wrapper around a FMOD channel, created when you play a `FmodSoundWrap` with `AudioManager` methods. It is designed to be backwards compatible with `UnityEngine.AudioSource`.

It is important to note that a channel will be automatically released by FMOD when it stops, either due to the sound finishing or you calling `Stop`. Afterwards, all operations on the channel will result in warnings in the Unity console. You can call `SetSoundEndCallback` to receive a callback when the sound finishes playing. Currently there is no way to check whether an `FmodChannelWrap` object contains a valid channel.

```
FmodSoundWrap sound
```

The sound that this channel is playing. Read only. This property has an alias `clip` for backwards compatibility.

```
bool isPlaying
```

Whether the channel is playing. Read only. Being paused is considered playing.

```
bool loop
```

Whether the channel loops.

```
float panStereo
```

The pan of the channel. -1 for left, 1 for right.

```
float pitch
```

The playback speed of the channel. 1 is normal speed. Changing speed will cause the pitch to change, hence the name.

```
float time
int timeSamples
```

The current time of the playback, in seconds and samples.

```
float volume
```

The volume of the channel.
* 0 is silent, 1 is full.
* Smaller 0 to invert the signal.
* Larger than 1 to amplify the signal.

```
void Pause()
void UnPause()
```

Pauses and unpauses playback.

```
void Play()
```

An alias for `UnPause`.

```
void Stop()
```

Stops playback. The callback set in `SetSoundEndCallback`, if any, will be called, then the channel will become invalid.

```
void SetSoundEndCallback(function callback)
```

Registers a callback to be called when the channel stops. The callback will be called with no parameters.

## Class `FmodSoundWrap`

A wrapper around a FMOD sound, created when you load audio files with `ThemeApi.IO` methods. It is designed to be backwards compatible with `UnityEngine.AudioClip`.

It is important to note that once a sound is loaded into memory, it stays in memory until you call `IO.ReleaseAudio`. Failure to release unused sounds, especially those loaded from files, may cause TECHMANIA to run out of memory and crash.

```
int channels
```

The number of audio channels (not to be confused with FMOD channels) in the sound. Read only.

```
int frequency
```

The frequency in Hz. Read only.

```
float length
int samples
```

The sound's length in seconds and samples. Both read only.

```
bool Equals(FmodSoundWrap other)
```

Returns whether this object and the other object refer to the same FMOD sound.

## Class `GameTimer`

Exposes the internal game timer. Access an instance via `tm.game.timer`. All fields are read-only.

### Time

```
float baseTime
```

The "base time" in seconds, which drives the backing track, BGA, hidden notes and auto-play notes.

```
float prevFrameBaseTime
```

Base time of the previous frame.

```
float gameTime
```

The "game time" in seconds, which is calculated by applying the note offset to base time. This drives the scanlines and playable notes.

```
int speedPercent
float speed
```

The game speed in integer percents (normal speed is 100) and float (normal speed is 1), respectively.

### Pulse, beat, scan

```
int pulsesPerScan
```

A helper with conversion between pulse and scan. Equals (pulses per beat) * (beats per scan).

```
float pulse
float beat
float scan
```

The current pulse, beat and scan number, calculated from `gameTime`. These are all internal number, meaning they all start from 0, despite the editor showing them in 1-index.

```
int intPulse
int intBeat
int intScan
```

`pulse`, `beat` and `scan` rounded down to the nearest integer, for convenience.

```
float prevFrameScan
int prevFrameIntScan
```

The `scan` and `intScan` of the previous frame. Some internal updates rely on this.

### Pattern border

```
int firstScan
int lastScan
```

The numbers of the first scan and last scan of the current pattern. Scan numbers start from 0.

```
float patternEndTime
```

The base time when the pattern ends, in seconds.

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
List<Subfolder> GetTrackSubfolders(string parent)
List<TrackInFolder> GetTracksInFolder(string parent)
List<ResourceWithError> GetTracksWithError(string parent)
```

Returns all [`Subfolder`](#class-globalresourcesubfolder), [`TrackInFolder`](#class-globalresourcetrackinfolder) or [`ResourceWithError`](#class-globalresourceresourcewitherror) objects in the specified parent folder, corresponding to, respectively, subfolders in it, tracks in it (but not in a subfolder), and tracks with errors (due to I/O errors, deserialization errors, etc.) in it.

`parent` should be the track root folder, available at `tm.paths.GetTrackRootFolder()`, or a subfolder of it.

If there are no subfolders / tracks / tracks with errors in `parent`, or `parent` does not exist, returns an empty list.

```
void ClearTrackList()
```

Clears the in-memory track list to release memory. After calling this, `GetTrackSubfolders`, `GetTracksInFolder` and `GetTracksWithError` will stop working.

```
bool anyOutdatedTrack
```

Whether there is any track in an outdated format on disk. Currently there is no way to query which specific track is outdated.

### Resource on setlists

```
List<Subfolder> GetSetlistSubfolders(string parent)
List<SetlistInFolder> GetSetlistsInFolder(string parent)
List<ResourceWithError> GetSetlistsWithError(string parent)
```

Returns all [`Subfolder`](#class-globalresourcesubfolder), [`SetlistInFolder`](#class-globalresourcesetlistinfolder) or [`ResourceWithError`](#class-globalresourceresourcewitherror) objects in the specified parent folder, corresponding to, respectively, subfolders in it, setlists in it (but not in a subfolder), and setlists with errors (due to I/O errors, deserialization errors, etc.) in it.

`parent` should be the setlist root folder, available at `tm.paths.GetSetlistRootFolder()`, or a subfolder of it.

If there are no subfolders / setlists / setlists with errors in `parent`, or `parent` does not exist, returns an empty list.

```
void ClearSetlistList()
```

Clears the in-memory setlist list to release memory. After calling this, `GetSetlistSubfolders`, `GetSetlistsInFolder` and `GetSetlistsWithError` will stop working.

```
bool anyOutdatedSetlist
```

Whether there is any setlist in an outdated format on disk. Currently there is no way to query which specific setlist is outdated.

```
Status SearchForPatternReference(Setlist.PatternReference reference, out TrackInFolder trackInFolder, out Pattern minimizedPattern)
```

Dereferences a [`Setlist.PatternReference`](#class-setlistpatternreference) by searching through all patterns loaded into memory for one that matches the reference's track GUID and pattern GUID.

If found, returns an OK status, a [`GlobalResource.TrackInFolder`](#class-globalresourcetrackinfolder) object describing the track, and the minimized pattern. See `TrackInFolder` for what "minimize" means.

If not found, returns a status with code `Status.Code.NotFound`, and both out parameters are null.

### Resource on themes

```
List<string> GetThemeList()
```

Returns the names of all available themes.

## Class `GlobalResource.Subfolder`

Describes a subfolder in some parent folder given to `GlobalResource.GetTrackSubfolders` or `GlobalResource.GetSetlistSubfolders`.

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

## Class `GlobalResource.SetlistInFolder`

Describes a setlist in some parent folder given to `GlobalResource.GetSetlistsInFolder`.

```
string folder
```

The full path to the folder that holds the `setlist.tech` file. File names in the setlist, such as eyecatch image and background image, should be concatenated to this folder to get the full path.

```
DateTime modifiedTime
```

The last modified time of `folder`. For newly unzipped folders, this is the time of unzipping.

```
Setlist setlist
```

The setlist loaded from `setlist.tech`. Different from tracks, setlists are loaded to memory in their entirety.

## Class `GlobalResource.TrackInFolder`

Describes a track in some parent folder given to `GlobalResource.GetTracksInFolder`.

```
string folder
```

The full path to the folder that holds the `track.tech` file. File names in the track, such as BGAs and keysounds, should be concatenated to this folder to get the full path.

```
DateTime modifiedTime
```

The last modified time of `folder`. For newly unzipped folders, this is the time of unzipping.

```
Track minimizedTrack
```

The minimized track loaded from `track.tech`. To save memory, TECHMANIA discards all notes, BPM events and time stops from tracks when building the track list. If you need the full track, call `tm.io.LoadFullTrack`.

## Class `GlobalResource.ResourceWithError`

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

## Class `Locale`

Describes a locale in [`ThemeApi.ThemeL10n`](#class-themeapithemel10n).

```
string languageName
```

The language's name, read from row 2 of the string table.

```
List<string> localizers
```

The list of localizer names, read from row 3 of the string table.

```
Dictionary<string, string> strings
```

All strings in the locale.

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
void ApplyVolumeSettings()
```

Applies current volume settings.

```
bool useAsio
```

Whether to use ASIO. Only available on Windows. Call `ApplyAsio()` to apply this.

```
void ApplyAsio()
```

Applies `useAsio`. Note that if the player doesn't have ASIO4ALL installed when turning this on, FMOD will not produce an error, but then all audio playback will be stuck at 0 seconds.

```
int audioBufferSize
int numAudioBuffers
```

The size (in samples) and quantity of audio buffers. The default is 1024 samples, 4 buffers. These are only applied on TECHMANIA startup. Refer to FMOD's [documentation](https://www.fmod.com/docs/2.02/api/core-api-system.html#system_setdspbuffersize) for more explanation on these settings.

### Appearance

```
string locale
```

The current locale. Refer to [`ThemeApi.ThemeL10n`](#class-themeapithemel10n) for more details on localization. If your theme uses `ThemeApi.ThemeL10n`, you need to call `tm.l10n.ApplyLocale()` to apply this.

Note that, even if your theme does not use `ThemeApi.ThemeL10n`, this setting still applies to things outside of your theme, such as the boot screen and the editor, but you don't need to call `tm.l10n.ApplyLocale()`.

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
string setlistsFolderLocation
string skinsFolderLocation
string themesFolderLocation
```

The full paths of the tracks / setlists / skins / themes folder, if `customDataLocation` is true. Call `tm.paths.ApplyCustomDataLocation()` to apply these.

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
const string kSetlistFilename = "setlist.tech"
```

The `track.tech` and `setlist.tech` filenames as constants.

### Resource folders

There is a set of track, setlist, skin and theme folders in an on-disk location (either next to TECHMANIA.exe or in custom data locations), and another set in "streaming assets". See [Folders and zips](../Folders_and_zips.md) for an explanation of streaming assets.

In most cases, TECHMANIA will conceptually merge contents in streaming assets into the on-disk location, so you do not need to care about streaming assets at all. If for any reason you need a path specifically in streaming assets, you can pass `streamingAssets = true` to the corresponding method.

```
void ApplyCustomDataLocation()
```

Call this after changing any option related to custom data locations to apply them.

```
string GetTrackRootFolder(bool streamingAssets = false)
string GetSetlistRootFolder(bool streamingAssets = false)
```

Returns the root folder for tracks / setlists. The track / setlist list in `GlobalResources` is built from this folder and its subfolders.

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
WindowsAndDeltas legacyRulesetOverride
List<WindowsAndDeltas> legacySetlistOverride
```

See [`PatternMetadata`](#class-patternmetadata) and [`WindowsAndDeltas`](#class-windowsanddeltas).

`legacySetlistOverride` contains 4 elements, one for each stage.

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

## Struct `Pattern.RadarDimension`

Describes one dimension of a radar.

```
float raw
int normalized
```

The raw and normalized values of this dimension. `normalized` is in \[0, 100\]. See [Radar](../Radar.md).

## Struct `Pattern.Radar`

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

```
void SaveToFile()
```

Saves the current records to disk.

### Setlists

```
SetlistRecord setlist.GetRecord(Setlist s, Options.Ruleset ruleset)
SetlistRecord setlist.GetRecord(Setlist s)
```

Retrieves the [`SetlistRecord`](#class-setlistrecord) for the specified [`Setlist`](#class-setlist) and ruleset.

The `ruleset` is the `Options.Ruleset` enum, not a `Ruleset` object. It is also optional. If omitted, this method will retrieve record for the currently selected ruleset. Note that no record will ever be created for the custom ruleset.

If no record exists for the specified setlist and ruleset, returns nil.

Technically these methods are on an internal field in `Records` named `setlist`, but for simplicity you can treat `setlist.` as part of the method name.

## Class `Ruleset`

Stores the actual numbers in a ruleset. Not to be confused with `Options.ruleset`, whose value is an enum `Ruleset`. Access the numbers in the currently selected ruleset via `tm.ruleset`.

```
string kVersion
```

The version of the serialization format. The current value is "3". Refer to [format version history](../Format_version_history.md) for more details.

```
WindowsAndDeltas windowsAndDeltas
```

The time windows and HP deltas when playing single patterns. See [Rulesets](../Rulesets.md).

```
List<WindowsAndDeltas> windowsAndDeltasSetlist
```

The time windows and HP deltas when playing setlists. Contains 4 elements, one for each stage. See [Rulesets](../Rulesets.md).

```
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

## Class `Setlist`

A named collection of patterns.

```
string kVersion
```

The version of the serialization format. The current value is "1". Refer to [format version history](../Format_version_history.md) for more details.

```
SetlistMetadata setlistMetadata
```

See [`SetlistMetadata`](#class-setlistmetadata).

```
List<Setlist.PatternReference> selectablePatterns
List<Setlist.HiddenPattern> hiddenPatterns
```

The selectable and hidden patterns in this setlist. Selectable patterns are references; hidden patterns contain a reference and a criteria. See [`Setlist.PatternReference`](#class-setlistpatternreference) and [`Setlist.HiddenPattern`](#class-setlisthiddenpattern).

## Class `Setlist.HiddenPattern`

Contains a hidden pattern in a setlist.

```
Setlist.PatternReference reference
```

A reference to the pattern.

```
Setlist.HiddenPatternCriteriaType criteriaType
Setlist.HiddenPatternCriteriaDirection criteriaDirection
int criteriaValue
```

The type, direction and value of the criteria. When TECHMANIA evaluates whether this hidden pattern should be chosen, it will take the actual value specified by the criteria type, and check whether it's in the specified direction (< or >) of the criteria value.

## Class `Setlist.PatternReference`

A reference to a pattern.

```
string trackTitle
string trackGuid
string patternName
int patternLevel
int patternPlayableLanes
string patternGuid
```

The identifier and metadata of the referenced pattern. Only `trackGuid` and `patternGuid` are actually used to find patterns; all other fields are for informational purposes.

## Class `SetlistMetadata`

```
string guid
string title
string description
string eyecatchImage
string backImage
ControlScheme controlScheme
```

See [setlist.tech specification](../setlist.tech_specification.md).

## Class `SetlistRecord`

Stores the record for a specific setlist under a specific ruleset. Retrieve records with `tm.records.setlist.GetRecord`.

```
string setlistGuid
List<string> patternGuids
List<string> patternFingerprints
Options.Ruleset ruleset
int score
PerformanceMedal medal
string gameVersion
```

The fields in the record. `patternGuids` and `patternFingerprints` have 4 elements each. All fields are read only.

```
string Rank()
```

Converts `score` to a letter rank. See [`ScoreKeeper`](#class-scorekeeper)``.ScoreToRankAssumingStageClear``.

## Class `SetlistScoreKeeper`

Keeps track of score, combo, HP and fever while playing a setlist. Access the `SetlistScoreKeeper` instance via `tm.game.setlist.scoreKeeper`. This instance is available in states `GameState.State.LoadComplete`, `GameState.State.Ongoing`, `GameState.State.Paused`, `GameState.State.PartialComplete` and `GameState.State.Complete`.

Internally, `SetlistScoreKeeper` maintains a list of up to 4 child `ScoreKeeper` instances, one for each stage. Most `SetlistScoreKeeper` fields work by aggregating the same field on all children `ScoreKeeper`s.

```
bool stageFailed
```

Whether the player has failed to clear the setlist due to losing all HP, or not meeting the HP threshold after a stage. If true, the score will be invalid.

### Accessing child `ScoreKeeper`s

```
ScoreKeeper GetScoreKeeperForStage(int stage)
```

Returns the child `ScoreKeeper` for the specified stage number in 0-index. Returns null if the stage hasn't started yet.

```
ScoreKeeper GetCurrentScoreKeeper()
```

A convenience method to return the child `ScoreKeeper` corresponding to the current stage.

### Score

```
int totalNotes
int totalFeverBonus
int maxScore
int NumNotesWithJudgement(Judgement judgement)
bool AllNotesResolved()
int ScoreFromNotes()
int ComboBonus()
int TotalScore()
PerformanceMedal Medal()
string ScoreToRankAssumingStageClear(int score)
string Rank()
```

The same fields in `ScoreKeeper` but for the entire setlist. See `ScoreKeeper` for explanations.

### Combo

```
int currentCombo
int maxCombo
```

The current combo and max combo of the setlist so far. Combo will not reset between stages.

### HP

```
int maxHp
int hp
```

The max HP and current HP in the setlist, respectively.

### Fever

```
ScoreKeeper.FeverState feverState
```

The current fever state. It will be reset to `Building` between stages.

```
float feverAmount
```

The current fever amount, in \[0, 1\]. It will not reset between stages.

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
Status.Code codeEnum
```

The status code, one of the following:
* `Status.Code.OK`: no error, operation successfully finished.
* `Status.Code.NotFound`: some file is not found.
* `Status.Code.IOError`: an I/O error when reading or writing a file.
* `Status.Code.FormatError`: the format of some file is invalid.
* `Status.Code.OtherError`: an error that does not fit any other category.

The `Status.Code` enum is exposed to Lua as `tm.enum.statusCode`.

```
string code
```

Returns `codeEnum` as a string. Deprecated; new code should use `codeEnum`.

```
bool Ok()
```

A shortcut to check whether the code is `OK`.

```
string errorMessage
string filePath
```

If the code is not `OK`, these fields may contain a more detailed error message and/or the path of the file that caused the error.

## Class `ThemeApi.ApplicationFocusEvent`

A custom event raised when the TECHMANIA window gains or loses focus.

This class inherits from `UnityEngine.UIElements.EventBase`.

```
bool focus
```

Whether the TECHMANIA window has focus.

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

Allows your theme to launch and exit the TECHMANIA editor (recall that the editor is outside of themes). Access an instance via `tm.editorInterface`.

### Launching and exiting

```
void LaunchOnTrack(string trackFolder)
void LaunchOnSetlist(string setlistFOlder)
```

Launches the editor on the track / setlist in the specified folder. When the user later exits the editor, TECHMANIA will call `onExit`.

```
string TrackToDirectoryName(string title, string artist)
string SetlistToDirectoryName(string title)
```

A utility method to generate a track / setlist folder name based on the title, artist (track only) and current time.

```
Status CreateNewTrack(string parentFolder, string title, string artist, out string newTrackFolder)
Status CreateNewSetlist(string parentFolder, string title, out string newSetlistFolder)
```

Attempts to create a new track / setlist with the specified title and artist (track only), in the specified parent folder. If successful, returns the new track / setlist's folder so you can pass it to `LaunchOnTrack` / `LaunchOnSetlist`. It also updates the track / setlist lists in `tm.resources`.

In Lua, these methods return 2 values: `local status, newTrackFolder = tm.editorInterface.CreateNewTrack(parentFolder, title, artist)`

```
function onExit
```

A callback that TECHMANIA will call when the user exits the editor by clicking the "back" button or "delete track / setlist" button. It will be called with no arguments.

Note that, if the user deletes a track or setlist, TECHMANIA will update the corresponding lists in `tm.resources`.

### Editor preview

```
function onPreview
```

A callback that TECHMANIA will call when the user enters editor preview by clicking the "preview" button in the pattern editor. It will be called with the following arguments:
- `string trackFolder`
- `Track track`
- `Pattern pattern`
- `int startingScan`

However, these arguments are only for informational purposes. TECHMANIA will handle all the game setup internally.

```
void ReturnFromPreview()
```

Ends the editor preview and returns to the editor.

## Class `ThemeApi.FrameUpdateEvent`

A custom event raised once every frame. Attaching callbacks to this event may affect performance.

This class has no members other than those inherited from `UnityEngine.UIElements.EventBase`.

## Class `ThemeApi.GameSetup`

Setup for the TECHMANIA game. Your theme should set up all fields before loading any pattern or setlist.

### Per-pattern setup

```
string trackFolder
string patternGuid
```

The track folder and pattern GUID of the pattern to load and play. `ThemeApi.GameState.BeginLoading()` will load the track and pattern specified here.

### Pattern common setup

```
function onStageClear
```

A callback that is called when the player completes the pattern and the game enters `Complete` state. The game will no longer accept input or play any background audio/video at this point. Parameter: [`ScoreKeeper`](#class-scorekeeper). Will not be called in setlists.

```
function onStageFailed
```

A callback that is called when the player fails the game due to losing to all HP and the game enters `Complete` state. The game will no longer accept input or play any background audio/video at this point. Parameter: [`ScoreKeeper`](#class-scorekeeper). Will not be called in setlists.

### Per-setlist setup

Technically these fields are on an internal field in `GameSetup` named `setlist`, but for simplicity you can treat `setlist.` as part of the field name.

```
bool setlist.enabled
```

Whether TECHMANIA should load the setlist specified in setlist setup, or the pattern specified in per-pattern setup. Remember to turn it off if the user leaves setlists and plays a single pattern.

```
string setlist.setlistFolder
```

The folder containing the `setlist.tech` file to load and play. `ThemeApi.GameState.setlist.Prepare()` will load the setlist specified here.

```
List<int> setlist.patternIndices
```

The indices (in 0-index) of selectable patterns to play in this setlist. Should contain exactly 3 elements.

```
int setlist.hiddenPatternIndex
```

The index (in 0-index) of the hidden pattern that TECHMANIA has chosen. Read only. Will only be available once TECHMANIA enters `PartialComplete` state after stage 3.

### Setlist common setup

```
function setlist.onPartialComplete
```

A callback that is called when the player completes stage 1, 2 or 3 with HP above threshold. TECHMANIA will be in `PartialComplete` state when this is called. Parameter: [`SetlistScoreKeeper`](#class-setlistscorekeeper).

```
function setlist.onHpBelowThreshold
```

A callback that is called when the player completes stage 1, 2 or 3 with HP below threshold. TECHMANIA will be in `Complete` state when this is called. Parameter: [`SetlistScoreKeeper`](#class-setlistscorekeeper).

```
function setlist.onSetlistAllClear
```

A callback that is called when the player completes stage 4, therefore completing the entire setlist. TECHMANIA will be in `Complete` state when this is called. Parameter: [`SetlistScoreKeeper`](#class-setlistscorekeeper).

```
function setlist.onSetlistFailed
```

A callback that is called when the player's HP hits 0 when playing a setlist. TECHMANIA will be in `Complete` state when this is called. Parameter: [`SetlistScoreKeeper`](#class-setlistscorekeeper).

### Common setup

These fields only need to be set up once, and will be reused between patterns / setlists.

```
VisualElementWrap bgContainer
VisualElementWrap gameContainer
VisualElementWrap vfxComboContainer
```

The [`VisualElementWrap`](#class-themeapivisualelementwrap)s that hold the pattern's background, the game elements (note, scanlines, input feedback, etc.), and VFX and combo text, respectively. The three groups of elements are rendered in separate containers so you can insert other layers between them.

`vfxComboContainer` is currently unused, but you should set it anyway, so in the future when TECHMANIA allows rendering VFX and combo text on UI Toolkit, it will work out of the box.

```
FmodSoundWrap assistTick
```

The sound to use for the "assist tick" modifier.

```
function onLoadProgress
```

A callback that is called when TECHMANIA makes any progress on loading the pattern. Parameter: [`LoadProgress`](#class-themeapigamesetuploadprogress).

```
function onLoadError
```

A callback that is called when TECHMANIA encounters an error when loading the pattern and enters `LoadError` state. Parameter: [`Status`](#class-status)

The only available state change at this point is `ThemeApi.GameState.Conclude()`.

```
function onLoadComplete
```

A callback that is called when TECHMANIA completes loading a pattern and enters `LoadComplete` state. It becomes possible for your theme to call `ThemeApi.GameState.Begin()`. Parameter: none.

```
function onUpdate
```

A callback that is called every frame when TECHMANIA is in `Ongoing` or `Paused` state. Parameter: [`GameTimer`](#class-themeapigametimer).

```
function onNoteResolved
```

A callback that is called every time a note is resolved. Parameter: [`Note`](#class-note), [`Judgement`](#enum-judgement), [`ScoreKeeper`](#class-scorekeeper).

```
function onAllNotesResolved
```

A callback that is called right after the last note in a pattern is resolved. The game may not yet be complete at this point. Parameter: [`ScoreKeeper`](#class-scorekeeper).

```
function onComboTick
```

A callback that is called every time an ongoing hold or drag note increments combo count, whether it updates the max combo or not. Parameter: current combo as `int`.

```
function onFeverReady
```

A callback that is called when fever is ready to be activated. Parameter: none.

```
function onFeverUnready
```

A callback that is called when fever is no longer ready to be activated, due to a MISS or BREAK. Parameter: none.

```
function onFeverActivated
```

A callback that is called when fever is activated, whether by the player or by the auto fever modifier. Parameter: none.

```
function onFeverUpdate
```

A callback that is called when the fever value changes, whether due to resolving a note, or due to the flow of time when fever is active. Parameter: current fever value as `float`, in \[0, 1\].

```
function onFeverEnd
```

A callback that is called when fever ends after running is duration. Parameter: fever bonus from this activation as `int`.

## Class `ThemeApi.GameSetup.LoadProgress`

Contains details on the progress of pattern loading.

```
string fileJustLoaded
```

The path of the most recently loaded file, **relative** to the track folder.

```
int filesLoaded
int totalFiles
```

The number of files loaded so far, and the total number of files to load.

## Class `ThemeApi.GameState`

Interface to the state machine of the TECHMANIA game. See [`ThemeApi.GameState.State`](#enum-themeapigamestatestate) for an explanation on all possible states. Access an instance via `tm.game`. Also contains APIs to control the game in specific states.

```
ThemeApi.GameState.State state
```

The current state. Read only.

### State transition

```
void BeginLoading()
```

Begins loading the game, using the track folder and pattern GUID in `tm.gameSetup`. Only callable from `Idle` state; transitions to `Loading` state.

During loading, the game will call `tm.gameSetup.onLoadProgress`. If loading succeeds, the game will transition to `LoadComplete` state and call `tm.gameSetup.onLoadComplete`. If loading fails, the game will transition to `LoadError` state and call `tm.gameSetup.onLoadError`.

```
void Begin()
```

Begins the game. Only callable from `LoadComplete` state; transitions to `Ongoing` state.

During the game, TECHMANIA will call various callbacks in `tm.gameSetup` related to notes, combo and fever. Once the game ends (either in stage clear or stage failed), the game will transition to `Complete` state and call `tm.gameSetup.onStageClear` or `tm.gameSetup.onStageFailed`.

```
void Pause()
```

Pauses the game. Only callable from `Ongoing` state; transitions to `Paused` state.

```
void Unpause()
```

Unpauses the game. Only callable from `Paused` state; transitions to `Ongoing` state.

```
void Conclude()
```

Concludes the current game and releases all related resources. Callable from any state; transitions to `Idle` state.

Note that `scoreKeeper` will no longer be available after this call.

### Game control

```
ScoreKeeper scoreKeeper
```

Provides the [`ScoreKeeper`](#class-scorekeeper) object for the current game. Only available in `LoadComplete`, `Ongoing`, `Paused` and `Complete` states.

```
GameTimer timer
```

Provides the [`GameTimer`](#class-gametimer) object for the current game. Only available in `LoadComplete`, `Ongoing` and `Paused` states.

```
void ActivateFever()
```

Activates fever. Only callable in `Ongoing` state, and when fever is ready.

### Background

```
void UpdateBgBrightness()
```

Applies the background brightness from the [`PerTrackOptions`](#class-pertrackoptions) corresponding to the current track. Only callable in `Ongoing`, `Paused` and `Complete` states.

```
void ResetElementSizes()
```

Resets the layout of internal elements in `tm.gameSetup.gameContainer` and `tm.gameSetup.gameContainer`. Call this after changing the layout either. Only callable in `Ongoing`, `Paused` and `Complete` states.

```
void StopAllGameAudio()
void StopBga()
```

Stops all game audio and BGA, respectively.

### Score and record

```
bool ScoreIsValid()
```

Returns whether the current score is valid. A score becomes invalid if:
* Stage failed, or
* Any special modifier is used, or
* A custom ruleset is used.

```
bool ScoreIsNewRecord()
```

Returns whether the current total score is a new record. If the score is not valid, this is always false. Only callable in `Complete` state.

```
void UpdateRecord()
```

Updates the record on the current pattern. Total score and medal are saved separately. Only callable in `Complete` state.

The update is only in memory; call `tm.records.SaveToFile()` to save the records to disk.

### Practice mode

All methods here are only callable in practice mode.

```
void JumpToScan(int scan)
```

Jumps to the specified scan. Even though the editor displays scan numbers in 1-index (ie. the first scan is scan 1), internal scan numbers are in 0-index, including the argument to this method.

```
void SetSpeed(int speedPercent)
```

Sets the game speed. `speedPercent` must be positive.

```
bool autoPlay
bool showHitbox
```

Gets or sets whether to enable auto play, and whether to show hitbox on notes, respectively.

### Setlist

Technically these fields are on an internal field in `GameState` named `setlist`, but for simplicity you can treat `setlist.` as part of the field name.

```
int setlist.currentStage
```

Read only. The number of current stage the player is on, in 0-index. It can be 0, 1 or 2 for selectable patterns, and 3 for hidden pattern. Incremented on each call to `setlist.LoadNextPattern()`.

```
SetlistScoreKeeper setlist.scoreKeeper
```

Provides access to [`SetlistScoreKeeper`](#class-setlistscorekeeper).

```
Status setlist.Prepare()
```

Loads the setlist specified in `tm.gameSetup.setlist` (but not any pattern in it) and transitions to `PreparedSetlist` state on success. Your theme should then call `tm.game.setlist.LoadNextPattern()` to begin loading the first pattern.

On error, returns a non-OK `Status` object.

The load is synchronous, so it may block on disk I/O for a bit.

```
void setlist.LoadNextPattern()
```

Begins to load the next pattern in the setlist and transitions to `Loading` state.

```
bool setlist.ScoreIsValid()
```

Returns whether the current score is valid. A score becomes invalid if:
* Stage failed, or
* Any special modifier is used, or
* A custom ruleset is used.

```
bool setlist.ScoreIsNewRecord()
```

Returns whether the current total score is a new record. If the score is not valid, this is always false. Only callable in `Complete` state.

```
void setlist.UpdateRecord()
```

Updates the record on the current setlist. Total score and medal are saved separately. Only callable in `Complete` state.

The update is only in memory; call `tm.records.SaveToFile()` to save the records to disk.

## Class `ThemeApi.IO`

Contains methods to load resources from either the theme or a file on disk. Access this class via `tm.io`.

Note that resources loaded from files will remain in memory until you release them. To avoid out-of-memory crashes, remember to release resources loaded from files (eg. eyecatches, preview tracks, preview BGAs) after you no longer need them. Resources loaded from the theme are meant to stay in memory for the entire TECHMANIA session, so you do not need to release them (except for videos; see `LoadVideoFromTheme`).

### Loading from the theme

```
string LoadTextFileFromTheme(string path)
UnityEngine.Texture2D LoadTextureFromTheme(string path)
FmodSoundWrap LoadAudioFromTheme(string path)
UnityEngine.TextCore.Text.FontAsset LoadFontFromTheme(string path)
```

Loads a text file / texture / sound / font from the theme, respectively. These methods are synchronous and return the resource immediately. The `path` argument should begin with `Assets/UI/`. On error, these methods return `nil`.

```
void LoadVideoFromTheme(string path, function callback)
```

Loads a video from the theme as a `VideoElement` object. The callback will be called with 2 arguments: a [`Status`](#class-status) object, and the loaded [`VideoElement`](#class-themeapivideoelement) object.

Note that, different from other forms of resources, `VideoElement` objects involve a Unity `GameObject` in addition to the video clip. This `GameObject` needs to be destroyed after you are done with the video, whether the video comes from the theme or a file. Therefore, you should call `ReleaseVideo` in both cases. Calling `ReleaseVideo` on a `VideoElement` loaded from the theme will not release the video clip.

### Loading from a file

```
bool FileExists(string path)
```

Returns whether a file exists at the specified path.

```
void LoadTextureFromFile(string path, function callback)
void LoadAudioFromFile(string path, function callback)
void LoadVideoFromFile(string path, function callback)
```

Loads a texture / sound / video from a file, respectively. These methods are asynchronous and may take a few frames to complete. To receive the loaded resource, pass in a callback that will be called when the loading is complete. The callback will be called with 2 parameters: a [`Status`](#class-status) object, and the resource object (if status is OK):

* `UnityEngine.Texture2D` for texture
* `FmodSoundWrap` for audio
* [`VideoElement`](#class-themeapivideoelement) for video

Remember to `Release` these resources after you are done with them.

```
void ReleaseTexture(UnityEngineTexture2D texture)
void ReleaseAudio(FmodSoundWrap sound)
void ReleaseVideo(VideoElement video)
```

Releases a texture / sound / video from memory, respectively.

```
Track LoadFullTrack(string path)
```

Loads the `track.tech` file at the specified path, and returns the deserialized [`Track`](#class-track) object. Since the track list in `GlobalResource` only contains minimized tracks, you can call this to load the full track if you need to access the notes.

```
void ReloadTrackList(function progressCallback, function completeCallback)
void UpgradeAllTracks(function progressCallback, function completeCallback)
```

Reloads the track list from disk / upgrades all tracks to the latest version. `ReloadTrackList` does not modify tracks on disk and only updates the track list in `GlobalResource`. `UpgradeAllTracks` modifies tracks on disk, so there is a risk of I/O errors.

Both methods take a while to complete. While the reload / upgrade is in progress, TECHMANIA will call `progressCallback` with the path of the track currently being loaded / upgraded, as a string. Once the reload / upgrade is complete, TECHMANIA will call `completeCallback` with a [`Status`](#class-status) object.

```
void ReloadSetlistList(function progressCallback, function completeCallback)
void UpgradeAllSetlists(function progressCallback, function completeCallback)
```

Similar to `ReloadTrackList` and `UpgradeAllTracks` but for setlists.

```
void ReloadNoteSkin(function progressCallback, function completeCallback)
void ReloadVfxSkin(function progressCallback, function completeCallback)
void ReloadComboSkin(function progressCallback, function completeCallback)
void ReloadGameUiSkin(function progressCallback, function completeCallback)
```

Reloads the specified type of skin from disk. The name of the skin to load is read from options.

All methods take a while to complete. While the reload is in progress, TECHMANIA will call `progressCallback` with the path of the file currently being loaded, as a string. Once the reload is complete, TECHMANIA will call `completeCallback` with a [`Status`](#class-status) object.

```
int numTexturesFromFile
int numSoundsFromFile
int numVideosFromFile
```

The number of textures, sounds and videos that are loaded from files and not yet released, respectively.

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

Provides common .NET string methods as static methods. Some instance methods are converted to static methods with the 1st argument being the string instance. Refer to [net table](#net-table) for why this is necessary. Access this class via `net.string`.

```
int Length(string s)
bool Contains(string s, string value)
bool EndsWith(string s, string value)
string DoubleToString(double d, string format)
string Format(string format, string arg)
string Format(string format, string arg1, string arg2)
string Format(string format, string arg1, string arg2, string arg3)
string Format(string format, string arg1, string arg2, string arg3, string arg4)
int IndexOf(string s, string value)
int IndexOf(string s, string value, int startIndex)
string Insert(string s, int startIndex, string value)
string Join(string separator, string[] values)
int LastIndexOf(string s, string value)
int LastIndexOf(string s, string value, int startIndex)
string PadLeft(string s, int totalWidth)
string PadLeft(string s, int totalWidth, string paddingChar)
string PadRight(string s, int totalWidth)
string PadRight(string s, int totalWidth, string paddingChar)
string Remove(string s, int startIndex, int count)
string Replace(string s, string oldValue, string newValue)
string[] Split(string s, string separator)
bool StartsWith(string s, string value)
string Substring(string s, int startIndex)
string Substring(string s, int startIndex, int length)
string ToLower(string s)
string ToUpper(string s)
string Trim(string s)
string TrimEnd(string s)
string TrimStart(string s)
```

Some examples of how to convert a .NET string instance method to a `StringWrap` static method:

.NET: `string s = "foo";`

Lua: `local s = "foo"`

.NET: `int l = s.Length;`

Lua: `local l = net.string.Length(s)`

.NET: `string paddedS = s.PadLeft(5);`

Lua: `local paddedS = net.string.PadLeft(s, 5)`

Some notes on specific methods:

* `DoubleToString`'s 2nd argument is the format string passed to .NET's `double.ToString`.
* For methods that deal with indices, since they are passed to .NET, remember that they are in 0-index.
* For `PadLeft` and `PadRight` that accept a `paddingChar`, since there is no character type in Lua, pass in a 1-character string.

## Class `ThemeApi.Techmania`

The entry point to TECHMANIA's data and types. Also contains miscellaneous methods that don't really fit into another type.

### Accessing the visual tree

```
ThemeApi.VisualElementWrap root
```

Provides the root element in the visual tree. From here you can query other elements in the tree with the `Q` method, such as `tm.root.Q("element-name")`.

Note that, when TECHMANIA transitions to and away from your theme (such as to the editor), it does so by setting the `display` property of the root element. It is adviced that you never change this property on the root element from your theme, in order to not interfere with this transition.

```
ThemeApi.VisualElementWrap WrapVisualElement(UnityEngine.UIElements.VisualElement e)
```

Creates a `VisualElementWrap` object around the specified `VisualElement` object. This is the reverse of `VisualElementWrap.inner`.

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
AudioManager audio
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
void OpenSelectFileDialog(string title,
    string currentDirectory,
    bool multiSelect,
    string[] supportedExtensionsWithoutDot,
    function callback)
```

Opens the operating system's "open file" dialog. If the user selects one or more files, calls the callback with an array of file paths. If the user cancels the dialog, does not call the callback.

`title` is the title of the dialog. `currentDirectory` is the directory that the dialog starts at; pass in "" to start from the working directory. `multiSelect` specifies whether the user can select multiple files. `supportedExtensionsWithoutDot` is an array of supported file extensions, all without the dot. These parameters only work on Windows.

```
void OpenSelectFolderDialog(string title,
    string currentDirectory,
    function callback)
```

Opens the operating system's "open folder" dialog. If the user selects a folder, calls the callback with its path. If the user cancels the dialog, does not call the callback.

`title` is the title of the dialog. `currentDirectory` is the directory that the dialog starts at; pass in "" to start from the working directory. These parameters only work on Windows.

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
bool InEditor()
```

Returns whether TECHMANIA is running inside the Unity editor.

```
ThemeApi.Techmania.Platform GetPlatformEnum()
```

Returns the current platform, one of `ThemeApi.Techmania.Platform.Windows`, `ThemeApi.Techmania.Platform.Linux`, `ThemeApi.Techmania.Platform.macOS`, `ThemeApi.Techmania.Platform.Android`, `ThemeApi.Techmania.Platform.iOS` and `ThemeApi.Techmania.Platform.Unknown`.

The `ThemeApi.Techmania.Platform` enum is exposed to Lua as `tm.enum.platform`.

```
string GetPlatform()
```

Returns `GetPlatformEnum()` as a string. Deprecated; new code should use `GetPlatformEnum()`.

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

A toolkit to help you localize your theme. Usage of this class is completely optional. Access this class via `tm.l10n`.

If you do use this class to localize your theme, you need to prepare a "string table" as a .csv file in your theme. Usage of some spreadsheet software (eg. Excel) is recommended to ensure correct format and escaping.

The string table should follow the following format:

* Row 1: empty cell, empty cell, one locale name per cell
    * We recommend that you use the same locale names as TECHMANIA itself: `,,en,zh-Hans,zh-Hant,ja,ko`, and add more if needed
* Row 2: empty cell, empty cell, one language name per cell
    * Example: `,,English,简体中文,繁體中文,日本語,한국어`
* Row 3: empty cell, empty cell, one comma-separated localizer name list per cell
    * Example: `,,N/A,zh-Hans localizer,zh-Hant localizer,"jp localizer 1, jp localizer 2","ko localizer 1, ko localizer 2"`
* Remaining rows: one string per row
    * Column 1: comment, unused by this class
    * Column 2: string key
    * Columns 3 and onward: the string content in each locale
    * Example: `The button in alert boxes,alert_ok,OK,确定,確定,OK,확인`

Each string is identified by the corresponding string key, so they should all be unique in the string table. There are two types of string keys:

1. Ones containing at least one `#`: each such key will be interpreted as a series of element names in the visual tree. When you call `ApplyLocale`, TECHMANIA will query each element name in succession, and overwrite the final element's text content with the string's content.
    * For example, if a row in the string table is `,#alert-box #ok-button,OK,确定,確定,OK,확인`, and the current locale is `zh-Hans`, upon `ApplyLocale` TECHMANIA will set `tm.root.Q("alert-box").Q("ok-button").text` to `确定`.
2. Ones containing no `#`: these keys carry no special meaning and will be ignored by `ApplyLocale`. You can still query the content of these keys with `GetString`.
    * These keys may follow any format of your liking, as long as they don't contain `#`.

```
void Initialize(string stringTable)
```

Initializes localization with the specified string table. You should pass in the string table's entire content, instead of a path. Load a text file from the theme with `tm.io.LoadTextFileFromTheme`.

```
Dictionary<string, Locale> GetAllLocales()
```

Returns a dictionary of all locales in the string table. You can enumerate this dictionary as a Lua table. See [`Locale`](#class-locale).

```
void ApplyLocale()
```

Applies the current locale in `tm.options.locale` to the theme, replacing the text content of all elements that have a corresponding row in the string table.

```
string GetString(string key)
```

Returns the string content of the specified key under the current locale. If the key is not found, returns an empty string.

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

A wrapper around Unity's video player that can play video onto a specified [`VisualElementWrap`](#class-themeapivisualelementwrap). [`ThemeApi.IO`](#class-themeapiio)'s video loading methods return videos as `VideoElement` objects.

Internally, the `VideoElement` manages a render texture, the video player plays onto it, and the target [`VisualElementWrap`]'s background image is set to it.

Remember to call `IO.ReleaseVideo` on the `VideoElement` after you are done with it, in order to release memory.

```
ThemeApi.VisualElementWrap targetElement
```

The `VisualElementWrap` to play the video on.

```
UnityEngine.Video.VideoPlayer player
```

Provides access to the underlying Unity video player.

```
UnityEngine.RenderTexture renderTexture
```

Provides access to the underlying render texture.

### Properties

```
float length
float time
```

The total length / current time of the video, both in seconds.

```
bool isPlaying
bool isPaused
```

Whether the video is playing / paused. Both are read only.

```
bool isLooping
```

Whether the video loops.

### Controls

```
void Play()
void Pause()
void Unpause()
void Stop()
```

Plays / pauses / unpauses / stops the video.

`Stop` is known to cause unexplained freezes in other videos at times. If you run into this, consider calling `Pause` instead.

```
void Dispose()
```

Deprecated; new code should call `IO.ReleaseVideo`.

```
void SetVideoEndCallback(function callback)
```

Registers a callback to be called when the video reaches its end. The callback will be called with no parameters.

For looping videos, this callback will be called every time the video reaches its end.

## Class `ThemeApi.VisualElementWrap`

A wrapper around Unity's `UnityEngine.UIElements.VisualElement` class, providing various additional functionalities. Each object corresponds to one element in the visual tree.

Note that it is possible to create different `VisualElementWrap` objects on the same element, therefore when testing for equality, call `Equals` instead of comparing the `VisualElementWrap` objects themselves.

```
UnityEngine.UIElements.VisualElement inner
```

Provides the internal `VisualElement` object that the `VisualElementWrap` object wraps around.

If you wish to do the reverse, ie. create a `VisualElementWrap` object on a `VisualElement` object, you can call `ThemeApi.Techmania.WrapVisualElement`:

```
local wrap = tm.WrapVisualElement(inner)
```

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

Registers a callback function for the specified event type. See [`EventType`](#enum-visualelementwrapeventtype) for all possible event types. The `data` parameter is optional, and can be of any Lua type. If `data` is set, it will be passed back to the callback when called.

You can register multiple handlers for each event type on each element. This is one of the additional features `VisualElementWrap` provides over `VisualElement`.

The callback will be called with 3 arguments:
* The `ThemeApi.VisualElementWrap` receiving the event
* The event object (see [`EventType`](#enum-visualelementwrapeventtype) for the type of this argument)
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
bool ContainsPointInScreenSpace(UnityEngine.Vector3 screenSpace)
```

Returns whether the element contains the specified point in screen space. If passing in a `Vector3`, the z coordinate is ignored.

Beware that when you get pointer positions from a UI Toolkit pointer event, they are not in screen space, so plugging them into this function will not work as expected. To check whether an element contains a pointer event's position:
* Check the `pointerId` field on the event to see whether it's from the mouse or a touch
* If mouse (`pointerId == 0`), use `unity.input.mousePosition` as the pointer position
* If touch (`pointerId > 0`), use `unity.input.GetTouch(pointerId - 1).position` as the pointer position

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

## Class `WindowsAndDeltas`

Describes the time windows and HP deltas of a ruleset or a legacy ruleset override.

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

See [Rulesets](../Rulesets.md).

```
bool HasAny()
```

A helper method to determine whether there is any value at all in this object.

# TECHMANIA enums

For more details on the modifier enums, see [Modifiers](../Modifiers.md). Also note that while most modifier enums have a `Normal` or `None` value, they are all different values, ie. not equal to each other.

## Enum `ControlScheme`

Describes one of TECHMANIA's supported control schemes.

```
Touch
Keys
KM
```

## Enum `CurveType`

Describes a drag note's curve type.

```
Bezier
BSpline
```

In Bezier, each point on the curve is controlled by the anchor before it, the right control point on that anchor, the anchor after it, and the left control point on that anchor. In B-spline, each point on the curve is controlled by two anchors before it, and two anchors after it.

See [Drag note curves](../Drag_note_curves.md) for more details.

## Enum `Judgement`

Describes the possible judgements that a note can be resolved in, depending on the player's timing.

```
RainbowMax
Max
Cool
Good
Miss
Break
```

## Enum `Modifiers.AssistTick`

```
None
AssistTick
AutoAssistTick
```

## Enum `Modifiers.ControlOverride`

```
None
OverrideToTouch
OverrideToKeys
OverrideToKM
```

## Enum `Modifiers.Fever`

```
Normal
FeverOff
AutoFever
```

## Enum `Modifiers.Keysound`

```
Normal
AutoKeysound
```

## Enum `Modifiers.Mode`

```
Normal
NoFail
AutoPlay
Practice
```

## Enum `Modifiers.NoteOpacity`

```
Normal
FadeOut
FadeOut2
FadeIn
FadeIn2
```

## Enum `Modifiers.NotePosition`

```
Normal
Mirror
```

## Enum `Modifiers.ScanDirection`

```
Normal
RR
LR
LL
```

## Enum `Modifiers.ScanlineOpacity`

```
Normal
Blink
Blink2
Blind
```

## Enum `Modifiers.ScanPosition`

```
Normal
Swap
```

## Enum `Modifiers.ScrollSpeed`

```
Normal
HalfSpeed
ShiftedHalfSpeed
```

## Enum `Modifiers.SuddenDeath`

```
Normal
SuddenDeath
```

## Enum `NoteType`

Describes all possible types of notes.

```
Basic
ChainHead
ChainNode
Hold
Drag
RepeatHead
RepeatHeadHold
Repeat
RepeatHold
```

Among the repeat types, `RepeatHead` and `RepeatHeadHold` are the two types of heads, and `Repeat` and `RepeatHold` are the two types of ticks that follow heads. See [Terminology](../Terminology.md).

## Enum `Options.Ruleset`

Describes the choices of rulesets. For the numbers and fields inside a ruleset, see [class `Ruleset`](#class-ruleset).

```
Standard
Legacy
Custom
```

## Enum `PerformanceMedal`

Describes the medals that the player may achieve after clearing a pattern.

```
NoMedal
AllCombo
PerfectPlay
AbsolutePerfect
```

* `AllCombo` requires no `Miss` or `Break`.
* `PerfectPlay` additionaly requires no `Cool` or `Good`.
* `AbsolutePerfect` additionaly requires no `Max`; all judgements must be `RainbowMax`.

## Enum `Setlist.HiddenPatternCriteriaType`

Describes the type of a hidden pattern's criteria.

```
Index
Level
HP
Score
Combo
MaxCombo
D100
```

See the setlist editor for explanations.

## Enum `Setlist.HiddenPatternCriteriaDirection`

Describes the direction of a hidden pattern's criteria.

```
SmallerThan
```

The actual value specified by the criteria type must be smaller than the criteria value.

```
LargerThan
```

The actual value specified by the criteria type must be larger than the criteria value.

## Enum `ScoreKeeper.FeverState`

Describes possible Fever states during gameplay.

```
Building
```

Fever is neither full or active. If fever is enabled, it will accumulate with each `RainbowMax` or `Max`, and decrease with each `Miss` or `Break`. If fever is disabled, it will remain at 0.

When fever accumulates to 1, it will go to `Ready` state.

```
Ready
```

Fever is full and ready to be activated. On `Miss` or `Break`, it will decrease and return to `Building` state.

```
Active
```

Fever is activated and decreasing with time. No judgement will affect its value or state. When fever is fully depleted, it will return to `Building` state.

## Enum `Status.Code`

Describes the result of an operation.

```
OK
NotFound
IOError
FormatError
OtherError
```

## Enum `ThemeApi.GameState.State`

Describes possible states of the TECHMANIA rhythm game. Query the current state with `tm.game.state`.

A diagram showing all states and possible transitions:

![TECHMANIA state machine](https://github.com/techmania-team/techmania-docs/assets/30272345/99f2ace1-b423-4b8c-93af-38f05b135089)

```
Idle
```

Waiting on the theme to fill `tm.gameSetup` and call either `tm.game.BeginLoading()` (transitions to `Loading`) for patterns or `tm.game.setlist.Prepare()` (transitions to `PreparedSetlist`) for setlists.

Any state can transition to `Idle` by calling `tm.game.Conclude()`.

```
PreparedSetlist
```

Setlist only. TECHMANIA has loaded the specified setlist, and is waiting on the theme to call `tm.game.setlist.LoadNextPattern()`, transitioning to `Loading`.

```
Loading
```

TECHMANIA is loading the pattern specified in `tm.gameSetup` and calling the callbacks to report progress. Automatically transitions to `LoadError` or `LoadComplete` depending on whether the load is successful.

```
LoadError
```

TECHMANIA encountered an error while loading the pattern and cannot continue.

```
LoadComplete
```

TECHMANIA successfully loaded the pattern and is now ready to begin. Call `tm.game.Begin()` to begin the game and transition to `Ongoing` state.

```
Ongoing
Paused
```

The game is ongoing / paused. Call `tm.game.Pause()` and `tm.game.Unpause()` to transition between these two states.

```
PartialComplete
```

Setlist only. The player has completed stages 1, 2 or 3 with HP above threshold. The game no longer updates or responds to input. Call `tm.game.setlist.LoadNextPattern()` to proceed, transitioning to `Loading`.

Note that, if the player completed stage 4, or completed stage 1/2/3 with HP below threshold, or hit 0 HP, TECHMANIA will transition to `Complete` state.

```
Complete
```

The game is complete, either by stage clear or stage fail. The game no longer updates or responds to input. Call `tm.game.Conclude()` to transition to `Idle`.

## Enum `ThemeApi.Techmania.Platform`

Describes the platform that TECHMANIA is built on.

```
Windows
Linux
macOS
Android
iOS
Unknown
```

## Enum `VisualElementWrap.EventType`

Describes the possible event types when registering an event callback in `ThemeApi.VisualElementWrap.RegisterCallback`. Each event type corresponds to a Unity type, which is the type of the 2nd argument passed to the callback.

|`EventType`|Unity type|
|--|--|
|`ChangeBool`|`UnityEngine.UIElements.ChangeEvent<bool>`|
|`ChangeInt`|`UnityEngine.UIElements.ChangeEvent<int>`|
|`ChangeFloat`|`UnityEngine.UIElements.ChangeEvent<float>`|
|`ChangeString`|`UnityEngine.UIElements.ChangeEvent<string>`|
|`KeyDown`|`UnityEngine.UIElements.KeyDownEvent`|
|`KeyUp`|`UnityEngine.UIElements.KeyUpEvent`|
|`PointerDown`|`UnityEngine.UIElements.PointerDownEvent`|
|`PointerUp`|`UnityEngine.UIElements.PointerUpEvent`|
|`PointerEnter`|`UnityEngine.UIElements.PointerEnterEvent`|
|`PointerLeave`|`UnityEngine.UIElements.PointerLeaveEvent`|
|`PointerOver`|`UnityEngine.UIElements.PointerOverEvent`|
|`PointerOut`|`UnityEngine.UIElements.PointerOutEvent`|
|`Click`|`UnityEngine.UIElements.ClickEvent`|
|`FrameUpdate`|`ThemeApi.FrameUpdateEvent`|
|`ApplicationFocus`|`ThemeApi.ApplicationFocusEvent`|

# On undocumented APIs

If you read TECHMANIA's source code, you may discover some APIs that are exposed to Lua, but not documented in this reference. Please assume all such omissions are intentional, and these APIs may stop working without notice in a future API version. Do not use undocumented APIs.
