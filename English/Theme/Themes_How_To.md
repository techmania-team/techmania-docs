# Themes How-to
This article aims to be a reverse index of the [scripting reference](Scripting_reference.md). Instead of listing APIs and telling you what they do, we will list some things you may want to do and point you towards the APIs to do that.

## Set aspect ratio / dynamic layout

* Create a `Panel Settings` asset in `Assets/UI`
* Select the asset and adjust `Scale Mode Parameters`
  * To make sure your theme fits the entire screen, set `Screen Match Mode` to `Match Width Or Height`. You will need to layout your elements in a way that works on all common aspect ratios. We recommend you to set `Match` to either 0 or 1, so your UI layout will have the exact width / height as `Reference Resolution`, and only the other dimension is dynamic.
  * To make sure your theme has a constant aspect ratio, set `Screen Match Mode` to `Expand`. There will be black borders around your theme if it doesn't match the user's display's aspect ratio.
* In script, load and apply the panel settings via `tm.SetPanelSettings`

## Listen to UI events

Use `VisualElementWrap.RegisterCallback`.

## Change styles of elements

Change styles on a `VisualElementWrap` via `style` field, though a few common ones have a shortcut on the `VisualElementWrap` itself. `style` is of type `UnityEngine.UIElements.IStyle`.

If changing some value inside `style` has no effect, you may need to instantiate new objects. The documentation on `style` provides an example.

## Animate elements

Most animations can be achieved by continuously changing some style on an element over multiple frames. If you are familiar with Unity coroutines, you can implement this in a similar style with `tm.StartCoroutine`.

## Load audio, texture and video

Use `tm.io`, of type `ThemeApi.IO`. Audio can be played with `tm.audio`, textures can be set to `backgroundImage` of `VisualElementWrap`s, and videos can be played with members in `VideoElement`.

## Retrieve track list

See `tm.resource`, of type `GlobalResource`.

## Options, modifiers and ruleset

These are available at `tm.options`, `tm.options.modifiers`, `tm.ruleset` respectively.

Read the documentation of `Options` carefully to understand which options need to be applied with a separate call.

## Per-theme options

Each theme gets its own dictionary to store any option it needs. Access it via `tm.options.GetPerTrackOptions`.

## Starting, stopping, controlling the game

* Fill in `tm.gameSetup` (of type `ThemeApi.GameSetup`)
  * Only track folder and pattern GUID are required. TECHMANIA will handle the loading of the pattern.
  * For setlists, fill in fields that start with `setlist`. Make sure `setlist.enabled` is set to true.
* Interact with the TECHMANIA state machine via methods in `tm.game` (of type `ThemeApi.GameState`)
  * Likewise, TECHMANIA will handle the background, score keeping and gameplay; your theme only needs to respond to events, such as fever being activated and note being resolved.
  * A typical workflow is:
    * Theme fills `tm.gameSetup` and calls `tm.game.BeginLoading()`
    * TECHMANIA loads the pattern and calls `tm.gameSetup.onLoadComplete`
    * Theme calls `tm.game.Begin()`
    * During game play, theme calls `tm.game.Pause()`, `tm.game.Unpause()` and `tm.game.ActivateFever()` as necessary
    * After stage clear or stage failed, TECHMANIA calls `tm.gameSetup.onStageClear` or `tm.game.StageFailed`
    * Theme calls `tm.game.Conclude()`
  * A typical workflow for setlists is:
    * Theme fills `tm.gameSetup` and calls `tm.game.setlist.Prepare()`
    * TECHMANIA loads the setlist
    * Theme calls `tm.game.setlist.LoadNextPattern()`
    * TECHMANIA loads the 1st pattern and calls `tm.gameSetup.onLoadComplete`
    * Theme calls `tm.game.Begin()`
    * During game play, theme calls `tm.game.Pause()`, `tm.game.Unpause()` and `tm.game.ActivateFever()` as necessary
    * After the player clears or fails a stage, TECHMANIA calls `tm.gameSetup.setlist.onPartialComplete`, `tm.gameSetup.setlist.onHpBelowThreshold` or `tm.gameSetup.setlist.onSetlistFailed`
    * If the player hasn't failed yet, theme calls `tm.gameSetup.LoadNextPattern()` for stage 2, and the previous few steps repeat for all 4 stages
    * After the setlist is complete, theme calls `tm.game.Conclude()`
* During the game, you can query information on the ongoing game from `tm.game.scoreKeeper` and `tm.game.timer`.
  * For setlists, use `tm.game.setlist.scoreKeeper` instead of `tm.game.scoreKeeper`

## Get and set records

To get the record on a pattern / setlist, call `tm.records.GetRecord` / `tm.records.setlist.GetRecord` and pass in the full pattern / setlist. Tracks from `tm.resources` are minimized and do not contain full patterns; you will need to manually load them with `tm.io.LoadFullTrack`.

When the game is in `Complete` state, you can call `tm.game.ScoreIsNewRecord()` to check if the player made a new record, and `tm.game.UpdateRecord()` to update the in-memory record. For setlists, call `tm.game.setlist.ScoreIsNewRecord()` and `tm.game.setlist.UpdateRecord()`.

## Localization

See `ThemeApi.ThemeL10n`.

## Enter the editor

See `ThemeApi.EditorInterface`.

## Display previews for skins / timing calibration

See `ThemeApi.CalibrationPreview` and `ThemeApi.SkinPreview`.

## Managing memory

The following resources need to be manually released after you are done with them in order to conserve memory:
* Textures loaded from files
* Sounds loaded from files
* Videos loaded from the theme
* Videos loaded from files

See `ThemeApi.IO`.