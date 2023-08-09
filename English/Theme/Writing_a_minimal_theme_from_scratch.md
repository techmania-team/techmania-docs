# Writing a minimal theme from scratch

This article aims to guide you to write a minimum viable TECHMANIA theme from scratch. Prerequisite: you have read the [Introduction](Introduction.md).

There will be a lot of **Tasks** that involve UI and/or coding. For these, we will first explain the goal, then provide the **Solution**, the UI setup / code that accomplishes that goal. You may choose to stop after reading the goal, then refer to your Unity  knowledge and/or the [Scripting reference](Scripting_reference.md) to come up with the solution yourself. Or you can simply follow / copy the provided solution to move forward faster. It's up to you.

## Environment setup

* Follow the "Getting started" section in [Introduction](Introduction.md). For step 6, since we are starting from scratch, delete every file other than `Assets/UI/.vscode` (this directory should be hidden in Unity anyway).
* Install Visual Studio Code, the recommended IDE for developing themes. Use Code to open the `Assets/UI` directory in your project.
* **Task 1** create an empty UXML document at `Assets/UI/MainTree.uxml`.
* **Task 2** create an empty text file at `Assets/UI/MainScript.txt`.

**Solution 1** this is best done within Unity. In the Project panel, navigate to `Assets/UI`, then right click - Create - UI Toolkit - UI Document. Name the new file `MainTree.uxml`.

**Solution 2** Unity doesn't provide the option to create a `.txt` file, so this is best done within VS Code. Click File - New Text File, then save as `MainScript.txt`. You can also do this in Windows File Explorer.

After the setup, enter play mode in Unity. If you did everything correctly, you should see the TECHMANIA boot screen (if you weren't using the default theme, now's your chance to revert), and after it finishes, a black screen. More importantly, no errors in the console.

## Hello World

Since the TECHMANIA theme API is not unlike an engine in itself, let's write a Hello World theme in it, as a programmer is wont to do.

**Task 3** Open `MainTree.uxml`, then add a Label and Button to the UI. Make sure the Label's text color is white so it is visible on a black background. Don't worry about other styles for now.

**Solution 3** When you double click a `.uxml` file, it will open in the UI Builder. From here, drag a Label from the Library panel into the Hierarchy to add one. Do the same for Button.

To style the Label, first select it, then find various style properties in the Inspector panel to the right. From here, find the "Color" property under "Text", and change it to white.

**Task 4** Open `MainScript.txt`, then write some script that changes the Label's text to "Hello world" when the user clicks the button. Hints:
* In order to access visual elements in scripts, you can name them in the UI builder.
* Use `tm.root` to access the visual tree's root, then the `Q` method to query its children.
* Use the `RegisterCallback` method to register an event hander on the `click` event.

**Solution 4**

First, return to UI Builder and name both the Label and the Button. In this solution we will call them `hello-label` and `hello-button`.

In `MainScript.txt`, write:

```
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

Let's start with the select track screen. We wish to display one button for every track in the track folder (ignore subfolders for now), and when the user clicks one, it brings them to the select pattern button.

There is one problem when designing the UI for this screen: at design time we don't know how many tracks the user has, so we can't just place some number of buttons and call it done. Instead, we have to prepare a "template" for a track button, and instantiate one instance for each track we find at runtime. In UXML, such a template takes the form of another `.uxml` document.

**Task 5** Prepare a UXML template named `Button.uxml` that contains a single button named `button`.

**Solution 5** Create `Button.uxml` the same way you created `MainTree.uxml`. Make sure it's also in `Assets/UI`. Then, open it, add a Button, and name it `button`.

**Task 6** In `MainTree.uxml`, add a full-screen visual element, named `select-track-screen`, as the select track screen. We will later add buttons to this visual element.

**Solution 6** Open `MainTree.uxml`, delete everything you added in the Hello world section, then add a Visual Element from the Library. Name it `select-track-screen`. By default, it should have Grow under Flex set to 1, meaning it will try to take up all space available to it, which in this case is the entire screen.

**Task 7** In `MainScript.txt`, write some script that acquires the track list from TECHMANIA, then instantiates an instance of `Button.uxml` under `select-track-screen` for each track. The button's text should be the corresponding track's title. Hints:
* The track list is available somewhere in `tm.resources`.
* Use the `InstantiateTemplate` method to instantiate a UXML document as a child of an element.

**Solution 7**
```
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