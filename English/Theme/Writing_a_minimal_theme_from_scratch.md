Applies to version: 2.2 (Theme API version 3)

# Writing a minimal theme from scratch

This article aims to guide you to write a minimum viable TECHMANIA theme from scratch. Prerequisite: you have read the [Introduction](Introduction.md).

There will be a lot of **Tasks** that involve UI and/or coding. For these, we will first explain the goal, then provide the **Solution**, the UI setup / code that accomplishes that goal. You may choose to stop after reading the goal, then refer to your Unity  knowledge and/or the [Scripting reference](Scripting_reference.md) to come up with the solution yourself. Or you can simply follow / copy the provided solution to move forward faster. It's up to you.

## Environment setup

* Follow the "Getting started" section in [Introduction](Introduction.md). For step 6, since we are starting from scratch, delete every file other than `Assets/UI/.vscode` (this directory should be hidden in Unity anyway).
* Install Visual Studio Code, the recommended IDE for developing themes. Use Code to open the `Assets/UI` directory in your project.

> **Task 1** create an empty UXML document at `Assets/UI/MainTree.uxml`.

> **Task 2** create an empty text file at `Assets/UI/MainScript.txt`.

> **Solution 1** this is best done within Unity. In the Project panel, navigate to `Assets/UI`, then right click - Create - UI Toolkit - UI Document. Name the new file `MainTree.uxml`.

> **Solution 2** Unity doesn't provide the option to create a `.txt` file, so this is best done within VS Code. Click File - New Text File, then save as `MainScript.txt`. You can also do this in Windows File Explorer.

After the setup, enter play mode in Unity. If you did everything correctly, you should see the TECHMANIA boot screen (if you weren't using the default theme, now's your chance to revert), and after it finishes, a black screen. More importantly, no errors in the console.

## Hello World

Since the TECHMANIA theme API is not unlike an engine in itself, let's write a Hello World theme in it, as a programmer is wont to do.

> **Task 3** Open `MainTree.uxml`, then add a Label and Button to the UI. Make sure the Label's text color is white so it is visible on a black background. Don't worry about other styles for now.

> **Solution 3** When you double click a `.uxml` file, it will open in the UI Builder. From here, drag a Label from the Library panel into the Hierarchy to add one. Do the same for Button.
>
> To style the Label, first select it, then find various style properties in the Inspector panel to the right. From here, find the "Color" property under "Text", and change it to white.

> **Task 4** Open `MainScript.txt`, then write some script that changes the Label's text to "Hello world" when the user clicks the button. Hints:
> * In order to access visual elements in scripts, you can name them in the UI builder.
> * Use `tm.root` to access the visual tree's root, then the `Q` method to query its children.
> * Use the `RegisterCallback` method to register an event hander on the `click` event.

> **Solution 4**
First, return to UI Builder and name both the Label and the Button. In this solution we will call them `hello-label` and `hello-button`.
>
> In `MainScript.txt`, write:

```lua
-- Promote the `tm` table to global scope
api = getApi(1)
tm = api.tm

-- Find the label and button
helloLabel = tm.root.Q("hello-label")
helloButton = tm.root.Q("hello-button")

-- Register callback for click event
helloButton.RegisterCallback(tm.enum.eventType.click, function()
    -- Change the label's text
    helloLabel.text = "Hello world!"
end)
```

When you are done, enter play mode and verify that the button works.

## Select track screen

Now let's begin making the actual theme. Since we are making a minimum viable theme, it will only include the following absolutely critical screens:
* Select track
* Select pattern
* Game
* Result

Let's start with the select track screen. We wish to display one button for every track in the track folder (ignore subfolders for now), and when the user clicks one, it brings them to the select pattern screen.

There is one problem when designing the UI for this screen: at design time we don't know how many tracks the user has, so we can't just place some number of buttons and call it done. Instead, we have to prepare a "template" for a track button, and instantiate one instance for each track we find at runtime. In UXML, such a template takes the form of another `.uxml` document.

> **Task 5** Prepare a UXML template named `Button.uxml` that contains a single button named `button`.

> **Solution 5** Create `Button.uxml` the same way you created `MainTree.uxml`. Make sure it's also in `Assets/UI`. Then, open it, add a Button, and name it `button`.

> **Task 6** In `MainTree.uxml`, add a full-screen visual element, named `select-track-screen`, as the select track screen. We will later add buttons to this visual element.

> **Solution 6** Open `MainTree.uxml`, delete everything you added in the Hello world section, then add a Visual Element from the Library. Name it `select-track-screen`. By default, it should have Grow under Flex set to 1, meaning it will try to take up all space available to it, which in this case is the entire screen.

> **Task 7** In `MainScript.txt`, write some script that acquires the track list from TECHMANIA, then instantiates an instance of `Button.uxml` under `select-track-screen` for each track. The button's text should be the corresponding track's title. Hints:
> * The track list is available somewhere in `tm.resources`.
> * Use the `InstantiateTemplate` method to instantiate a UXML document as a child of an element.

> **Solution 7**
```lua
api = getApi(1)
tm = api.tm

-- Get the select track screen
selectTrackScreen = tm.root.Q("select-track-screen")

-- Get the track list
trackList = tm.resources.GetTracksInFolder(tm.paths.GetTrackRootFolder())

-- Enumerate the track list
for _, trackInFolder in ipairs(trackList) do
    -- Instantiate button
    local instance = selectTrackScreen.InstantiateTemplate("Assets/UI/Button.uxml")
    -- Get a reference to the button
    local button = instance.Q("button")
    -- Get the track title
    local trackTitle = trackInFolder.minimizedTrack.trackMetadata.title
    -- Change the button's text
    button.text = trackTitle
end
```

## Select pattern screen

For the select pattern screen, we will again display one button per pattern. The button text should include the pattern's number of playable lanes, difficulty level, and name. But before we begin...

> **Task 8** Hide the select track screen so we can work on the select pattern screen. But we also need the select track screen to be visible when the player starts the theme. Update `MainScript.txt` to accomplish that.

> **Solution 8** In `MainTree.uxml`, select `select-track-screen`, then in the Inspector, turn off Display under Display.
>
> Since `MainScript.txt` will be executed right after a theme is loaded, we can add a line to make sure `select-track-screen` is displayed at that time:

```lua
-- (right after defining selectTrackScreen in Solution 7)
selectTrackScreen.display = true
```

Now we can start creating the select pattern screen.

> **Task 9** Add another full-screen visual element, named `select-pattern-screen`, next to `select-track-screen`.

> **Solution 9** Similar to Solution 6, or you can right click `select-track-screen`, select "Duplicate", then rename the new element. Make sure `select-track-screen` and `select-pattern-screen` are next to each other, instead of one containing the other.

Next, we would instantiate buttons for patterns the same way as tracks, but to do that we need a track. We will only know which track to display patterns for once the player makes a selection at the select track screen. At the same time, the select track screen should disappear, giving way to the select pattern screen. These are all parts of the "transition" from select track screen to select pattern screen.

> **Task 10** Update `MainScript.txt` so that when the player clicks a track button, the following happens in succession:
> * The select track screen disappears
> * The select pattern screen appears (it should also be hidden at startup)
> * Instantiate one button for each pattern, and the button's text contains the corresponding pattern's number of playable lanes, difficulty level and name
>
> Hints:
> * All track buttons can use the same click callback, but they should still somehow let the select pattern screen know which track is selected.
> * You can take advantage of the 3rd parameter in `RegisterCallback` to pass different data to the same callback.

> **Solution 10** Notice we wrote a separate function to handle the instantiation of pattern buttons. This is to avoid writing code that is too deeply nested.
```lua
api = getApi(1)
tm = api.tm

selectTrackScreen = tm.root.Q("select-track-screen")
selectTrackScreen.display = true

-- New code: find select pattern screen
selectPatternScreen = tm.root.Q("select-pattern-screen")
selectPatternScreen.display = false

trackList = tm.resources.GetTracksInFolder(tm.paths.GetTrackRootFolder())
for _, trackInFolder in ipairs(trackList) do
    local instance = selectTrackScreen.InstantiateTemplate("Assets/UI/Button.uxml")
    local button = instance.Q("button")
    local trackTitle = trackInFolder.minimizedTrack.trackMetadata.title
    button.text = trackTitle

    -- New code: register callback
    button.RegisterCallback(tm.enum.eventType.click, function(_, _, minimizedTrack)
        selectTrackScreen.display = false
        selectPatternScreen.display = true
        DisplayPatternsForTrack(minimizedTrack)
    end, trackInFolder.minimizedTrack)
end

-- New code: display patterns in the specified track
function DisplayPatternsForTrack(minimizedTrack)
    for _, pattern in ipairs(minimizedTrack.patterns) do
        -- Instantiate button
        local instance = selectPatternScreen.InstantiateTemplate("Assets/UI/Button.uxml")
        local button = instance.Q("button")

        -- Collect metadata and build the text on the button
        local metadata = pattern.patternMetadata
        local patternText = tostring(metadata.playableLanes) .. "L | Level " .. tostring(metadata.level) .. " | " .. metadata.patternName
        button.text = patternText
    end
end
```

Now enter the play mode again to verify that you can click a track button to see the patterns in it.

## Game screen

For the game screen in our minimal theme, we will show an HP bar, the current score, the game itself, and nothing else. Even then, this will require a little bit of UI layout.

When laying out a game screen, you need to prepare 3 visual elements for TECHMANIA to render various elements into: a background layer for BGA, a game layer for notes and scanlines, a VFX layer for VFX and combo text. Once you pass these  to the theme API, TECHMANIA will take over and you don't need to care about anything happening in them.

With these in mind, let's layout our game screen.

> **Task 11** Hide the select pattern screen so we can work on the game screen. Add a 3rd full-screen element called `game-screen`. Inside, set up 3 full-screen elements with the following name. Make sure they are all direct children to `game-screen` (ie. siblings of each other), and are all displayed.
> * `bg-layer`
> * `game-container`
> * `vfx-layer`
>
> Hint: you can use styles under "Position" to make sure these elements all take up the entire screen.

> **Solution 11** Elements with a Grow of 1 will take up as much space as available to it, but when 3 elements all try to grow, they end up each taking 1/3 of the available space.
>
> To force an element to take up the entirety of its parent's space and ignore its siblings, we need to set the following styles under Position:
> * Position: Absolute
> * Left, Top, Right, Bottom: 0px
>
> To do this to 3 elements is a bit repetitive, so we recommend setting up a USS class and applying it to all 3 elements. To do that, go to the StyleSheets panel at the top left of UI Builder, and type `.fully-expand` into the "Add new selector" text box. This will create a USS class called `fully-expand`. We currently don't have a USS file attached to `MainTree.uxml`, so UI Builder will create one. Save it as `Assets/UI/Style.uss`.
>
> Now, click the `.fully-expand` selector we created, and adjust its style the same way as with an element. Set up position, left, top, right and bottom.
>
> To apply a USS class to an element, select the element, then enter the class name (without the dot) into the text box in StyleSheet. Do this to `bg-layer`, `game-container` and `vfx-layer`.

In our theme, both the background layer and VFX layer will take up the entire screen, but the game layer will share the space within `game-container` with an HUD, containing the HP bar and score display. To keep things simple, the entire HUD will be the HP bar, with the score overlaid on top.

> **Task 12** Within `game-container`, create two elements: `hp-bar`, taking up 50 pixels of height and all available width, and `game-layer`, taking up the remaining height. Make sure `hp-bar` is aligned to the top.
>
> Within `hp-bar`, create the following elements:
> * `hp-fill`, filling up 70% of `hp-bar`'s width from the left
> * `score`, a Label, taking up the entire space of `hp-bar`
>
> Also feel free to set the background colors of `hp-bar` and `hp-fill`, as well as text color of `score`, to your liking.
>
> Hint: tinker with styles under Flex and Size to achieve the layout we need.

> **Solution 12** Set the following styles or USS classes:
> * `hp-bar`
>   * Under Size, set Height to 50px
>   * Under Flex, right click Grow and click Unset to reset it to 0, or it will take up more than 50px
>   * Under Background, set Color to any color you like
> * `hp-fill`
>   * Under Size, set Width to 70%
>   * Under Background, set Color to any color you like
> * `score`
>   * Apply the `fully-expand` class
>   * Under Text, set Color to any color you like
> * `game-layer`
>   * It should have Grow 1 by default, so it will naturally take up all space under `hp-bar`

With the layout complete, let's move on to scripting. First is the transition from select pattern screen.

> **Task 13** Update `MainScript.txt` so that:
> * The game screen is hidden at start
> * When the user clicks a pattern button, the select pattern disappears, then the game screen appears

> **Solution 13**
```lua
-- ... existing code omitted ...

gameScreen = tm.root.Q("game-screen")
gameScreen.display = false

function DisplayPatternsForTrack(minimizedTrack)
    for _, pattern in ipairs(minimizedTrack.patterns) do
        -- ... existing code omitted ...

        button.RegisterCallback(tm.enum.eventType.click, function()
            selectPatternScreen.display = false
            gameScreen.display = true
        end)
    end
end
```

TECHMANIA requires your theme to fill in the "game setup" before you can start a game, so let's take a look at those. According to the documentation on `ThemeApi.GameSetup`, the majority of setup is under "Common setup", and only need to be done once.

> **Task 14** Add script to set the following fields in `tm.gameSetup`. Make sure these are only set once when the theme is loaded.
> * `bgContainer`, `gameContainer`, `vfxComboContainer`: these are the elements `bg-layer`, `game-layer` and `vfx-layer` we prepared earlier.
> * `onLoadComplete`, `onNoteResolved`, `onStageCleared`, `onStageFailed`: set up an empty Lua function as handlers for now, we will fill in these functions later. All other `onSomething` fields will be ignored for this theme.

> **Solution 14** Add the following to `MainScript.txt`, somewhere after `gameScreen` is assigned:
```lua
function CommonGameSetup()
    tm.gameSetup.bgContainer = gameScreen.Q("bg-layer")
    tm.gameSetup.gameContainer = gameScreen.Q("game-layer")
    tm.gameSetup.vfxComboContainer = gameScreen.Q("vfx-layer")
    tm.gameSetup.onLoadComplete = function() end
    tm.gameSetup.onNoteResolved = function() end
    tm.gameSetup.onStageClear = function() end
    tm.gameSetup.onStageFailed = function() end
end
CommonGameSetup()
```

The other part of game setup is the per-pattern fields. For these, we need the track and pattern that the user selected at the select track screen and select pattern screen, respectively.

> **Task 15** Update the click handlers of track buttons and pattern buttons so that they set `tm.gameSetup.trackFolder` and `tm.gameSetup.patternGuid`, respectively.

> **Solution 15** In the track button's click handler, we used to pass in `trackInFolder.minimizedTrack`, but the track folder is in `trackInFolder` itself, so we need to pass the entire `trackInFolder` to the handler.
```lua
button.RegisterCallback(tm.enum.eventType.click, function(_, _, trackInFolder)
    selectTrackScreen.display = false
    selectPatternScreen.display = true
    tm.gameSetup.trackFolder = trackInFolder.folder  -- new
    DisplayPatternsForTrack(trackInFolder.minimizedTrack)
end, trackInFolder)
```
> In the pattern button's click handler, we also need to pass in the pattern in order to get the GUID from its metadata.
```lua
button.RegisterCallback(tm.enum.eventType.click, function(_, _, pattern)
    selectPatternScreen.display = false
    gameScreen.display = true
    tm.gameSetup.patternGuid = pattern.patternMetadata.guid  -- new
end, pattern)
```

Now we have completed all necessary setup. When it comes to actually starting, controlling and stopping the game, we need to interact with the TECHMANIA state machine, via `tm.game`, of type `ThemeApi.GameState`.

When TECHMANIA launches, we are in the `Idle` state. Once we fill in the game setup and call `tm.game.BeginLoading()`, we enter `Loading` state and wait for the game to load. Once the load is complete, TECHMANIA will enter `LoadComplete` state and call `tm.gameSetup.onLoadComplete`. Typically this is our cue to call `tm.game.Begin()` to start the game and enter `Ongoing` state. 

> **Task 16** Update `MainScript.txt` so that:
> * After the pattern button's click handler sets up `tm.gameSetup.patternGuid`, call `tm.game.BeginLoading()`
> * In the handler of `tm.gameSetup.onLoadComplete()`, call `tm.game.Begin()`

> **Solution 16**
>
```lua
function CommonGameSetup()
    tm.gameSetup.bgContainer = gameScreen.Q("bg-layer")
    tm.gameSetup.gameContainer = gameScreen.Q("game-layer")
    tm.gameSetup.vfxComboContainer = gameScreen.Q("vfx-layer")
    tm.gameSetup.onLoadComplete = function() tm.game.Begin() end  -- new
    tm.gameSetup.onNoteResolved = function() end
    tm.gameSetup.onStageClear = function() end
    tm.gameSetup.onStageFailed = function() end
end

button.RegisterCallback(tm.enum.eventType.click, function(_, _, pattern)
    selectPatternScreen.display = false
    gameScreen.display = true
    tm.gameSetup.patternGuid = pattern.patternMetadata.guid
    tm.game.BeginLoading()  -- new
end, pattern)
```

If you enter play mode now, you should be able to click a pattern and start playing it. However the HP bar and score display remain still and do not reflect the player's performance. Let's fix that.

There are two points at which you should update the HP bar and score: at the beginning of a pattern, and every time a note is resolved. At the beginning, the player has full HP and 0 points; after resolving a note, the HP and score can be read from the `ScoreKeeper` object that TECHMANIA passes into `tm.gameSetup.onNoteResolved`.

After we have these numbers, we should also reflect them in the UI. For the HP bar, we adjust the width of `hp-fill` according to the percentage of HP the player has. For score, we can simply update `score`'s text.

> **Task 17** Write a function that takes an HP percentage and a score, and updates UI accordingly.
>
> Hint: change a style of an element via the `style` field. Look up the `IStyle` interface in Unity documentation to see what fields are in it. Also read the first few sections of [scripting reference](Scripting_reference.md) to see how to construct objects, as well as how certain Unity types are exposed to Lua.

> **Solution 17**
```lua
function UpdateGameHUD(hpPercent, score)
    gameScreen.Q("hp-fill").style.width = unity.styleLength.__new(unity.length.__new(hpPercent, unity.enum.lengthUnit.Percent))
    gameScreen.Q("score").text = tostring(score)
end
```

> **Task 18** Call the function you just wrote at two places: just before `tm.game.BeginLoading()`, and in the handler of `tm.gameSetup.onNoteResolved`.

> **Solution 18**
```lua
-- At the top
api = getApi(1)
tm = api.tm
unity = api.unity  -- new

-- ... existing code omitted ...

function CommonGameSetup()
    tm.gameSetup.bgContainer = gameScreen.Q("bg-layer")
    tm.gameSetup.gameContainer = gameScreen.Q("game-layer")
    tm.gameSetup.vfxComboContainer = gameScreen.Q("vfx-layer")
    tm.gameSetup.onLoadComplete = function() tm.game.Begin() end
    tm.gameSetup.onNoteResolved = function(_, _, scoreKeeper)  -- new
        local hpPercent = scoreKeeper.hp * 100 / scoreKeeper.maxHp
        UpdateGameHUD(hpPercent, scoreKeeper.ScoreFromNotes())
    end
    tm.gameSetup.onStageClear = function() end
    tm.gameSetup.onStageFailed = function() end
end
CommonGameSetup()

-- ... existing code omitted ...

-- In DisplayPatternsForTrack
button.RegisterCallback(tm.enum.eventType.click, function(_, _, pattern)
    selectPatternScreen.display = false
    gameScreen.display = true
    tm.gameSetup.patternGuid = pattern.patternMetadata.guid
    UpdateGameHUD(100, 0)  -- new
    tm.game.BeginLoading()
end, pattern)
```

Now when you enter play mode, the HP bar and score display should respond to gameplay. Great!

We are almost done with the game screen. Note there are two pieces of `gameSetup` we still haven't filled: `onStageClear` and `onStageFailed`. `onStageClear` will go to the result screen so we will deal with that later.

When the player's HP hits zero and they are not in no fail mode, TECHMANIA will halt the game, enter `Complete` state, and call `tm.gameSetup.onStageFailed`. For our minimum theme, we simply return to the select track screen. Don't forget to call `tm.game.Conclude()` to return the state machine to `Idle` state.

> **Task 19** Add script to do the following in the `tm.gameSetup.onStageFailed` handler:
> * Call `tm.game.Conclude()`
> * Hide the game screen
> * Show the select track screen

> **Solution 19**
```lua
-- In CommonGameSetup()
tm.gameSetup.onStageFailed = function()
    tm.game.Conclude()
    gameScreen.display = false
    selectTrackScreen.display = true
end
```

Now enter play mode, start a pattern, then fail it, and you should return to the select track screen.

## Select pattern screen, again

Before we move on to result screen, there is an issue with the select pattern screen that we need to fix. It will happen when you select a track and pattern, fail it, then select another track.

> **Task 20** Figure out what is the issue with select pattern screen and fix it.

> **Solution 20** When you select a second track, patterns from both the first and second track will appear on the select pattern screen.
>
> This is because we only ever add buttons to the select pattern screen; we should also remove buttons from the previous track. Thankfully, since the select pattern screen contains nothing but pattern buttons, we can fix the issue simply by wiping out all pattern buttons before instantiating new ones.
```lua
function DisplayPatternsForTrack(minimizedTrack)
    selectPatternScreen.RemoveAllChildren()  -- new
    for _, pattern in ipairs(minimizedTrack.patterns) do
        -- ... existing code omitted ...
    end
end
```

## Result screen

To keep the result screen minimal, we will only display the following:
* Total score
* Max combo
* Rank
* A button to return to select track screen

As always, let's begin with UI layout.

> **Task 21** Hide `game-screen`. Add another top-level full-screen element named `result-screen`. In it, add the following 4 elements:
> * A Label named `total-score`
> * A Label named `rank`
> * A Label named `max-combo`
> * A button named `proceed-button`, with the text "Proceed"
>
> Make sure all Labels are white so they are visible on a black background.

> **Solution 21** At this point this should be trivial. Feel free to set up a new USS class named `white-text` and apply to the Labels.

Then, the transition from game screen. This transition happens on stage clear, so it will be in `tm.gameSetup.onStageClear`'s handler. While we are here, why not also set up the transition from result screen to select track screen, which happens when the user clicks the Proceed button.

> **Task 22** Update `MainScript.txt` so that `result-screen` is hidden at the start. Add code in `tm.gameSetup.onStageClear`'s handler that:
> * hides `game-screen`
> * shows `result-screen`
>
> Also add code so that when `proceed-button` is clicked:
> * hide `result-screen`
> * show `select-track-screen`

> **Solution 22**
```lua
-- ... existing code omitted ...

resultScreen = tm.root.Q("result-screen")
resultScreen.display = false

-- ... existing code omitted ...

tm.gameSetup.onStageClear = function()
    gameScreen.display = false
    resultScreen.display = true
end

-- ... existing code omitted ...

resultScreen.Q("proceed-button").RegisterCallback(tm.enum.eventType.click, function()
    resultScreen.display = false
    selectTrackScreen.display = true
end)
```

You may remember that when handling `tm.game.onStageFailed`, we called `tm.game.Conclude()` to return the state machine to `Idle` state. But we don't do that immediately when handling `tm.game.onStageClear`. This is intentional, as we need the result numbers from `tm.game.scoreKeeper`, which will become unavailable in `Idle` state. We still need to conclude the game, but only after we gathered all the data we need from `tm.game.scoreKeeper`.

Let's do exactly that. When transitioning to the result screen, we gather data from `tm.game.scoreKeeper`, display them in the elements, then conclude the game.

> **Task 23** Add code to `tm.gameSetup.onStageClear`'s handler that:
> * Reads the total score from `tm.game.scoreKeeper` and displays it on `total-score`
> * Reads the rank from `tm.game.scoreKeeper` and displays it on `rank`
> * Reads the max combo from `tm.game.scoreKeeper` and displays it on `max-combo`
> * Calls `tm.game.Conclude()`

> **Solution 23**
```lua
tm.gameSetup.onStageClear = function()
    gameScreen.display = false
    resultScreen.display = true

    -- New code below
    local scoreKeeper = tm.game.scoreKeeper
    resultScreen.Q("total-score").text = "Total score: " .. tostring(scoreKeeper.TotalScore())
    resultScreen.Q("rank").text = "Rank: " .. scoreKeeper.Rank()
    resultScreen.Q("max-combo").text = "Max combo: " .. tostring(scoreKeeper.maxCombo)
    tm.game.Conclude()
end
```

And that's it! Enter play mode and enjoy your first theme from start to finish!

## Full solution

`MainTree.uxml`

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements" xmlns:uie="UnityEditor.UIElements" xsi="http://www.w3.org/2001/XMLSchema-instance" engine="UnityEngine.UIElements" editor="UnityEditor.UIElements" noNamespaceSchemaLocation="../../UIElementsSchema/UIElements.xsd" editor-extension-mode="False">
    <Style src="project://database/Assets/UI/Style.uss?fileID=7433441132597879392&amp;guid=10b3506755352f74388355c040965a49&amp;type=3#Style" />
    <ui:VisualElement name="select-track-screen" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0); display: none;" />
    <ui:VisualElement name="select-pattern-screen" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0); display: none;" />
    <ui:VisualElement name="game-screen" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0); display: none;">
        <ui:VisualElement name="bg-layer" class="fully-expand" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0);" />
        <ui:VisualElement name="game-container" class="fully-expand" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0);">
            <ui:VisualElement name="hp-bar" style="background-color: rgb(0, 0, 0); height: 50px;">
                <ui:VisualElement name="hp-fill" style="background-color: rgb(56, 142, 60); width: 70%; flex-grow: 1;" />
                <ui:Label tabindex="-1" text="234564" display-tooltip-when-elided="true" name="score" class="fully-expand white-text" />
            </ui:VisualElement>
            <ui:VisualElement name="game-layer" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0);" />
        </ui:VisualElement>
        <ui:VisualElement name="vfx-layer" class="fully-expand" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0);" />
    </ui:VisualElement>
    <ui:VisualElement name="result-screen" style="flex-grow: 1; background-color: rgba(0, 0, 0, 0);">
        <ui:Label tabindex="-1" text="Label" display-tooltip-when-elided="true" name="total-score" class="white-text" />
        <ui:Label tabindex="-1" text="Label" display-tooltip-when-elided="true" name="rank" class="white-text" />
        <ui:Label tabindex="-1" text="Label" display-tooltip-when-elided="true" name="max-combo" class="white-text" />
        <ui:Button text="Proceed" display-tooltip-when-elided="true" name="proceed-button" />
    </ui:VisualElement>
</ui:UXML>
```

`Button.uxml`

```xml
<ui:UXML xmlns:ui="UnityEngine.UIElements" xmlns:uie="UnityEditor.UIElements" xsi="http://www.w3.org/2001/XMLSchema-instance" engine="UnityEngine.UIElements" editor="UnityEditor.UIElements" noNamespaceSchemaLocation="../../UIElementsSchema/UIElements.xsd" editor-extension-mode="False">
    <ui:Button text="Button" display-tooltip-when-elided="true" name="button" />
</ui:UXML>

```

`Style.uss`

```css
.fully-expand {
    position: absolute;
    left: 0;
    top: 0;
    right: 0;
    bottom: 0;
}

.white-text {
    color: rgb(255, 255, 255);
}
```

`MainScript.txt`

```lua
api = getApi(1)
tm = api.tm
unity = api.unity

selectTrackScreen = tm.root.Q("select-track-screen")
selectTrackScreen.display = true

selectPatternScreen = tm.root.Q("select-pattern-screen")
selectPatternScreen.display = false

gameScreen = tm.root.Q("game-screen")
gameScreen.display = false

resultScreen = tm.root.Q("result-screen")
resultScreen.display = false

function CommonGameSetup()
    tm.gameSetup.bgContainer = gameScreen.Q("bg-layer")
    tm.gameSetup.gameContainer = gameScreen.Q("game-layer")
    tm.gameSetup.vfxComboContainer = gameScreen.Q("vfx-layer")
    tm.gameSetup.onLoadComplete = function() tm.game.Begin() end
    tm.gameSetup.onNoteResolved = function(_, _, scoreKeeper)
        local hpPercent = scoreKeeper.hp * 100 / scoreKeeper.maxHp
        UpdateGameHUD(hpPercent, scoreKeeper.ScoreFromNotes())
    end
    tm.gameSetup.onStageClear = function()
        gameScreen.display = false
        resultScreen.display = true

        local scoreKeeper = tm.game.scoreKeeper
        resultScreen.Q("total-score").text = "Total score: " .. tostring(scoreKeeper.TotalScore())
        resultScreen.Q("rank").text = "Rank: " .. scoreKeeper.Rank()
        resultScreen.Q("max-combo").text = "Max combo: " .. tostring(scoreKeeper.maxCombo)
        tm.game.Conclude()
    end
    tm.gameSetup.onStageFailed = function()
        tm.game.Conclude()
        gameScreen.display = false
        selectTrackScreen.display = true
    end
end
CommonGameSetup()

trackList = tm.resources.GetTracksInFolder(tm.paths.GetTrackRootFolder())
for _, trackInFolder in ipairs(trackList) do
    -- Instantiate track button
    local instance = selectTrackScreen.InstantiateTemplate("Assets/UI/Button.uxml")
    local button = instance.Q("button")
    local trackTitle = trackInFolder.minimizedTrack.trackMetadata.title
    button.text = trackTitle

    button.RegisterCallback(tm.enum.eventType.click, function(_, _, trackInFolder)
        selectTrackScreen.display = false
        selectPatternScreen.display = true
        tm.gameSetup.trackFolder = trackInFolder.folder
        DisplayPatternsForTrack(trackInFolder.minimizedTrack)
    end, trackInFolder)
end

function DisplayPatternsForTrack(minimizedTrack)
    selectPatternScreen.RemoveAllChildren()
    for _, pattern in ipairs(minimizedTrack.patterns) do
        -- Instantiate pattern button
        local instance = selectPatternScreen.InstantiateTemplate("Assets/UI/Button.uxml")
        local button = instance.Q("button")

        -- Collect metadata and build the text on the button
        local metadata = pattern.patternMetadata
        local patternText = tostring(metadata.playableLanes) .. "L | Level " .. tostring(metadata.level) .. " | " .. metadata.patternName
        button.text = patternText

        button.RegisterCallback(tm.enum.eventType.click, function(_, _, pattern)
            selectPatternScreen.display = false
            gameScreen.display = true
            tm.gameSetup.patternGuid = pattern.patternMetadata.guid
            UpdateGameHUD(100, 0)
            tm.game.BeginLoading()
        end, pattern)
    end
end

function UpdateGameHUD(hpPercent, score)
    gameScreen.Q("hp-fill").style.width = unity.styleLength.__new(unity.length.__new(hpPercent, unity.enum.lengthUnit.Percent))
    gameScreen.Q("score").text = tostring(score)
end

resultScreen.Q("proceed-button").RegisterCallback(tm.enum.eventType.click, function()
    resultScreen.display = false
    selectTrackScreen.display = true
end)
```

## Next steps

Feel free to use the minimal theme as a starting point when building your dream theme. There are obviously many pieces missing in this minimal theme, such as:

* Options menu
* Pausing
* Fever
* Error handling

Consult [the scripting reference](Scripting_reference.md), or ask in the community, if you need help. For releasing your theme, refer to instructions in the [introduction](Introduction.md) page.

We look forward to your creativity. Good luck!