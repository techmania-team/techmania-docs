# TECHMANIA Theme API scripting reference

Applies to API version: 1

# `getApi`

`getApi` is the only function that the Theme API adds to the global scale. It returns a table with the following values:

|Key|Value|
|--|--|
|`"tm"`|An object of type `Techmania`|
|`"net"`|A Lua table exposing some .Net types|
|`"unity"`|A Lua table exposing some Unity types|
|`"util"`|A Lua table exposing some utility types|

The rest of this document assumes you have promoted these values to the global scale:

```
api = getApi(1)
tm = api.tm
net = api.net
unity = api.unity
util = api.util
```

## Exposing types

When some value in the API "exposes a type", you are able to access static members on the type via that value. For example, `unity.time` exposes Unity's `UnityEngine.Time` class, which has a static field called `deltaTime`, telling you the time elapsed since the last frame. To access it from Lua:

```
local deltaTime = unity.time.deltaTime
```

Note that Moonsharp, TECHMANIA's Lua interpreter, will convert `ref` and `out` parameters in C# methods to additional return values. For another example, `net.int` exposes .NET's `System.Int32` class, which has a static method `bool TryParse(string s, out int result)`. It tries to parse the input string into an integer, and writes the result to the `result` parameter if successful, and also returns a boolean indicating whether the conversion is successful. To access it from Lua:

```
local convertSuccess, result = net.int.TryParse("123")
```

## Exposing enums

When some value in the API "exposes an enum", treat each value in the enum as a static value. For example, `tm.enum.ruleset` exposes the TECHMANIA `Options.Ruleset` enum with values `Standard`, `Legacy` and `Custom`. You can use it like:

```
local ruleset = tm.enum.ruleset.Standard
if (ruleset == tm.enum.ruleset.Legacy) then ... end
```

## `tm` object

Refer to class Techmania for reference on members of this object, except `enum`.

## `tm.enum` table

`tm.enum` exposes the following TECHMANIA enums. Refer to the ["Enums"](#enums) section for reference on these enums.

|Key|Value|
|--|--|
|`eventType`|Enum `VisualElementWrap.EventType`|
|`ruleset`|Enum `Options.Ruleset`|
|`gameState`|Enum `GameState.State`|
|`controlScheme`|Enum `ControlScheme`|
|`noteType`|Enum `NoteType`|
|`curveType`|Enum `CurveType`|
|`noteOpacity`|Enum `Modifiers.NoteOpacity`|
|`scanlineOpacity`|Enum `Modifiers.ScanlineOpacity`|
|`scanDirection`|Enum `Modifiers.ScanDirection`|
|`notePosition`|Enum `Modifiers.NotePosition`|
|`scanPosition`|Enum `Modifiers.ScanPosition`|
|`fever`|Enum `Modifiers.Fever`|
|`keysound`|Enum `Modifiers.Keysound`|
|`assistTick`|Enum `Modifiers.AssistTick`|
|`mode`|Enum `Modifiers.Mode`|
|`controlOverride`|Enum `Modifiers.ControlOverride`|
|`scrollSpeed`|Enum `Modifiers.ScrollSpeed`|
|`judgement`|Enum `Judgement`|
|`performanceMedal`|Enum `PerformanceMedal`|
|`feverState`|Enum `ScoreKeeper.FeverState`|

## `net` table

## `unity` table

## `unity.enum` table

## `util` table

# Classes

Classes within this document are sorted alphabetically, but properties and methods within each class are grouped by usage. To quickly find references for a class, press Ctrl+F and search for "class \<classname\>".

# Enums