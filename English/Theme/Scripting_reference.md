# TECHMANIA Theme API scripting reference

Applies to API version: 1

When reading on Github, you can click the menu button to the top left to reveal a table of contents.

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

Allows the playback of audio clips. You can access an `AudioSourceManager` object via `tm.audio`.

### Playing audio

```
UnityEngine.AudioSource PlayMusic(UnityEngine.AudioClip clip,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
UnityEngine.AudioSource PlayKeysound(UnityEngine.AudioClip clip,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
UnityEngine.AudioSource PlaySfx(UnityEngine.AudioClip clip,
    float startTime = 0f,
    int volumePercent = 100,
    int panPercent = 0)
```

Plays the specified audio clip on the music / keysound / SFX channel, and returns the `AudioSource` playing the clip. Please note that TECHMANIA only allows playing one music clip at a time. You can load an audio clip from the theme with `tm.io.LoadAudioFromTheme`, or from the disk with `tm.io.LoadAudioFromFile`.

The clip will start playing from `startTime` seconds. `volumePercent` is between 0 and 100. `panPercent` is between -100 (left) and 100 (right).

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

## Class `HoldNote`

## Class `LegacyRulesetOverride`

## Class `Modifiers`

## Class `Note`

## Class `NoteSkin`

One of the 4 types of skins.

Refer to [Skins](../Skins.md#note-skin) for an explanation of these fields.

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
string stringValue
```

For elements that allow setting a string value (`DropdownField`, `TextField`), this is the current value. Setting it will trigger a change event.

```
string boolValue
```

For elements that allow setting a boolean value (`Toggle`), this is the current value. Setting it will trigger a change event.

```
void SetValueWithoutNotify(string newValue)
void SetValueWithoutNotify(float newValue)
void SetValueWithoutNotify(bool newValue)
```

Allows setting numeric, string or boolean value without triggering change events.

```
void ScrollTo(ThemeApi.VisualElementWrap child)
```

For `ScrollView`, this makes the element scroll to the specified element.

### Event handling

```
void RegisterCallback(ThemeApi.VisualElementWrap.EventType eventType,
    function callback,
    DynValue data)
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

Returns a list of all direct children.

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

```
UnityEngine.Vector2 ScreenSpaceToLocalSpace(UnityEngine.Vector2 screenSpace)
```

Converts a point in the screen space to the element's local space. When you query from `UnityEngine.Input` the coordinates of the mouse cursor or a touch, they are in the screen space.

```
bool ContainsPointInScreenSpace(UnityEngine.Vector2 screenSpace)
```

Returns whether the element contains the specified point in screen space.

## Class `TimeStop`

## Class `Track`

## Class `TrackMetadata`

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
