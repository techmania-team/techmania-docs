Applies to version: 2.2 (Theme API version 3)

# Introduction to TECHMANIA themes

Starting with 2.0, themes make up the entirety of TECHMANIA's UI (excluding the boot sequence and editors). Players can swap between themes to customize their UI. This article gives an overview on themes from a technical perspective, as well as how to develop them.

## Prerequisites

In order to develop a theme you need to be familiar with:
- Unity
  - Able to navigate the Unity editor and documentation
  - Understand the game loop
  - Familiar with common utility classes, such as `Time` and `Input`
- UI Toolkit
  - Know UXML and USS, and understand how they work
  - Able to edit UXML and USS with UI Builder
  - Able to debug UI with UI Debugger
- Lua
  - If you are familiar with C# but not Lua, here's [a quick guide](C%23_guide_to_Lua.md) to get started

Familiarity with web development is not required but it can be helpful, as many parallels can be drawn between a web page and a theme:
- HTML <--> UXML
- CSS <--> USS
- JavaScript <--> Lua

## What is a theme

From a technical perspective, a theme is a **Unity asset bundle** that:

- Contains a UXML document named `Assets/UI/MainTree.uxml`
- Contains a Lua script named `Assets/UI/MainScript.txt`
- Contains any other resource referenced by them inside `Assets/UI`, such as text files, images, audio clips, videos, fonts, UXML documents, USS files and Lua scripts.

Note that your Lua scripts must be in a file format that Unity recognizes as a ["Text Asset"](https://docs.unity3d.com/2022.2/Documentation/Manual/class-TextAsset.html) in order to be included in asset bundles. Therefore, you should write Lua scripts in `.txt` files, instead of `.lua`.

At runtime, after TECHMANIA finishes its boot sequence and loads the theme, it will:

- Display the UXML document in `Assets/UI/MainTree.uxml`
  - The root VisualElement will be accessible from your script as `tm.root`
- Execute the Lua script in `Assets/UI/MainScript.txt`
  - This is the only time that TECHMANIA proactively executes scripts in a theme; all other script execution will be based on events. Your script should set up all necessary event handlers on this initial execution.
  - You can spread your script across multiple files and call `tm.ExecuteScriptFromTheme` from `MainScript.txt`.

## Getting started

1. Go to the TECHMANIA release page, find the TECHMANIA version you wish to develop for, and download the source code from its release.
  - If you use git to clone the TECHMANIA repo, make sure you clone at the exact commit that release is created on, as newer commits may contain work-in-progress API changes.
2. Follow instructions in the "Making your own build" section in [TECHMANIA's readme](https://github.com/techmania-team/techmania#making-your-own-builds) to set up skins, so that your build is playable.
3. Install [Unity Hub](https://unity.com/download) and use it to open the project. Unity Hub will detect the Unity version of this project; you must install the same Unity version in order to open the project.
4. In Unity, open the scene `Assets/Scenes/Main.unity` if not already open.
5. Find the `Assets/UI` folder, which currently holds the official default theme.
6. If you wish to start from scratch, delete everything in `Assets/UI` except `Assets/UI/.vscode`, then create `Assets/UI/MainTree.uxml` and `Assets/UI/MainScript.txt`. Or you can keep the default theme and start by modifying it. Either way, make sure every resource that the theme references is inside `Assets/UI`. Do not modify anything else in the project.

## Testing and releasing

If you wish to test your theme, simply enter play mode in the Unity editor. You'll need to revert TECHMANIA to use the default theme if it's currently using another one.

Under the hood, there is an editor script that builds an asset bundle from the contents of `Assets/UI` whenever you enter play mode. This asset bundle can be found at `Assets/AssetBundles/default` (no extension). TECHMANIA itself is set up to load the default theme from that asset bundle if running in the Unity editor. There is also a script that will copy it to `<build folder>/Themes/Default.tmtheme` when you build the project, so you can continue testing your theme in a standalone player.

When you have finished developing your theme, copy `Assets/AssetBundles/default` and rename it to `<your theme name>.tmtheme`, then you can release it! Make sure to tell your users which TECHMANIA version and platform it's built for.

There is no trivial way to unpack an asset bundle into files, so you may want to release the source code alongside the theme if you wish to allow others to contribute, or base their themes on yours.

Asset bundles are platform-specific. To build asset bundles for other platforms, click the Window menu in Unity, then TECHMANIA - Build AssetBundles (custom platform)...

## Visual Studio Code

You are strongly encouraged to use [Visual Studio Code](https://code.visualstudio.com/) as the IDE when writing scripts for your theme. The `Assets/UI` folder contains a pre-made Code workspace setup in `Assets/UI/.vscode`, providing the following features:

* The workspace is set up to associate `.txt` files with Lua
* The workspace will recommend you to install the MoonSharp Debug extension, if not already installed
* The workspace contains a launch configuration that attaches the MoonSharp debugger to Unity, allowing you to debug your scripts

## Lua environment and script debugging

TECHMANIA uses [MoonSharp](https://www.moonsharp.org/) as its Lua interpreter. The environment that your scripts will run in is set up as a "soft sandbox", meaning most Lua standard libraries (such as `table`, `string` and `math`) are available, but you will not have access to the underlying OS.

If you use Visual Studio Code, a fully featured debugger is available, allowing you to set break points, step through lines, inspect local variables and more. To use it:

* Enter play mode in Unity.
* As TECHMANIA finishes loading the theme, watch the console for a message saying "Started MoonSharp debug server at port \<port number\>." The port is usually 41912.
* If the port is not 41912, open `UI/.vscode/launch.json` in Code, and change `"debugServer"`'s value to the port from the previous step.
* Launch the "MoonSharp Attach" configuration. The debugger should now be attached to the running theme.

If your script is split between multiple files, make sure to execute other files with `tm.ExecuteScriptFromTheme` instead of `tm.ExecuteScript`. The former passes the script files' paths to MoonSharp, allowing the debugger to find these source files.

Regardless of your choice of IDE, another debugging tool is available: the Lua enrivonment wires the `print` function to Unity's `Debug.Log`, so when your script calls `print`, you can find the result in Unity's console.

## UI debugging

To debug UI, use UI Toolkit's debugger, available at Window - UI Toolkit - Debugger. Once TECHMANIA loads and displays the theme, choose "Panel Settings" from the top-left dropdown to display the visual tree of your theme.

## `getApi`, API version and top-level tables

TECHMANIA provides a broad set of APIs for your script to interact with, allowing your theme to manipulate the visual tree, read tracks and patterns, read or write records, control the game, and more.

These APIs are organized as Lua tables, and you can retrieve them by calling `getApi(version)` with the API version number (as a number, not string) you wish to use. Consult TECHMANIA's release notes to see which releases support which API versions. At the time of writing, TECHMANIA 2.0 supports API version 1.

Once an API version is released it will never change. Whenever a new TECHMANIA release modifies the API, it will increment the version number. We strive to make API changes as backwards-compatible as possible, allowing newer TECHMANIA releases to support older API versions, but we cannot make guarantees. The `getApi(version)` function is the only thing we guarantee to never change across API versions.

To keep the global scope clean, `getApi` is the only thing TECHMANIA Theme API adds to the global scope. Refer to the [scripting reference](Scripting_reference.md) for an explanation on its return value.

## Class, object and userdata

When the Theme API exposes a C# class or object to Lua, it becomes a value of type userdata, but for all practical purposes you can use it as an object. As in, you can use the fields, properties and/or methods on the userdata value, and they will be wired to the underlying class or object. Therefore, the Theme API documentation will disregard the lack of object-oriented terms in Lua and introduce classes, objects, fields, properties and methods as themselves; it's up to you to translate them to Lua syntax.

## UI Toolkit limitations

* UI Toolkit does not currently support custom shaders or blend modes.
* UI Toolkit does not currently support non-rectangular masks. One workaround is to draw texture on a custom mesh, using related functions in `VisualElementWrap`.

## Next steps

- [Writing a minimal theme from scratch](Writing_a_minimal_theme_from_scratch.md)
- [Themes how-to](Themes_How_To.md)
- [Scripting reference](Scripting_reference.md)
