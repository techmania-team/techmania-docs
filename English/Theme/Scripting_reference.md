# TECHMANIA Theme API scripting reference

Applies to API version: 1

When reading on Github, you can click the menu button to the top left to reveal a table of contents.

# `getApi`

`getApi` is the entry point to TECHMANIA Theme API. It is also the only thing that the Theme API adds to the global scope. It returns a table with the following values:

|Key|Value|
|--|--|
|`tm`|An object of type `ThemeApi.Techmania`|
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

The `tm` object gives you access to various TECHMANIA types. Refer to class `ThemeApi.Techmania` for reference on members of this object, except `enum`.

## `tm.enum` table

`tm.enum` exposes the following TECHMANIA enums. Refer to the ["TECHMANIA enums"](#techmania-enums) section for reference on these enums.

|Key|Value|
|--|--|
|`eventType`|`VisualElementWrap.EventType`|
|`ruleset`|`Options.Ruleset`|
|`gameState`|`GameState.State`|
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
|`stringDict`|`System.Collections.Generic.Dictionary<System.String, System.String>`|

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

## Class `BpmEvent`

## Class `ComboSkin`

## Class `DragNode`

## Class `DragNote`

## Class `FloatPoint`

## Class `GameUISkin`

## Class `GlobalResource`

## Class `HoldNote`

## Class `LegacyRulesetOverride`

## Class `Modifiers`

## Class `Note`

## Class `NoteSkin`

## Class `Options`

## Class `Paths`

## Class `Pattern`

## Class `PatternMetadata`

## Class `PerTrackOptions`

## Class `Record`

## Class `Records`

## Class `Ruleset`

## Class `SpriteSheet`

## Class `Status`

## Class `ThemeApi.CalibrationPreview`

## Class `ThemeApi.EditorInterface`

## Class `ThemeApi.GameSetup`

## Class `ThemeApi.GameState`

## Class `ThemeApi.IO`

## Class `ThemeApi.SkinPreview`

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
void SetThemeStyleSheet(string path)
```

Sets the theme style sheet of the UI document. `path` is the path towards the `.tss` file, starting with `Assets/UI`. The default theme uses this method to customize the style of dropdowns.

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
void ExecuteScript(string script)
```

Executes the input string as Lua script. Using this together with `ThemeApi.IO`, you can split your script across multiple files, and execute them from the main script: `tm.ExecuteScript(tm.io.LoadTextFileFromTheme("Assets/UI/another script file.txt"))`

```
int StartCoroutine(function function)
```

Executes the Lua function as a coroutine and returns its coroutine ID, which can be later passed to `StopCoroutine` to stop execution. Within a coroutine, you can call `coroutine.yield()` to pause execution, and TECHMANIA will automatically resume execution on the next frame.

```
void StopCoroutine(int id)
```

Stops the specified coroutine if it's current running. If the specified coroutine is not running, the behavior is undefined.

### Miscellaneous

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
void Quit()
```

Quits TECHMANIA. If running in the Unity editor, this will quit the play mode.

```
Table enum
```

A Lua table exposing various TECHMANIA enums. Refer to [`tm.enum` table](#tmenum-table) for reference.

## Class `ThemeApi.ThemeL10n`

## Class `ThemeApi.UQueryStateWrap`

## Class `ThemeApi.VideoElement`

## Class `ThemeApi.VisualElementWrap`

## Class `TimeStop`

## Class `Track`

## Class `TrackMetadata`

## Class `VfxSkin`

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
