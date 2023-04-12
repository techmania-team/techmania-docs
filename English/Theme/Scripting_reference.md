# TECHMANIA Theme API scripting reference

Applies to API version: 1

# `getApi`

`getApi` is the entry point to TECHMANIA Theme API. It is also the only thing that the Theme API adds to the global scope. It returns a table with the following values:

|Key|Value|
|--|--|
|`tm`|An object of type `Techmania`|
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

When some value in the API "exposes a type", you are able to access the constructor and static members on the type via that value. For example, `unity.time` exposes Unity's `UnityEngine.Time` class, which has a static field called `deltaTime`, telling you the time elapsed since the last frame. To access it from Lua:

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

The `tm` object gives you access to the core data and control of TECHMANIA's various mechanisms. Refer to class Techmania for reference on members of this object, except `enum`.

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

Classes within this document are sorted alphabetically, but properties and methods within each class are grouped by usage. To quickly find references for a class, press Ctrl+F and search for "class \<classname\>".

# TECHMANIA enums