Applies to version: 0.6

This page explains the files and folders important to the game, and how the game handles zip archives.

# TL;DR

* Pattern makers: please include the track folder when zipping up your work.

Right:
```
new track.zip
    Artist - Title - 12345
        track.tech
        keysound.ogg
        bga.mp4
        ...
```

Wrong:
```
new track.zip
    track.tech
    keysound.ogg
    bga.mp4
    ...
```

* Players: drop zips into the `Tracks` folder next to `TECHMANIA.exe`, and the game will automatically extract them. You can create subfolders inside `Tracks` to organize tracks too.

# Folders

The following 4 folders are of significance:
* `Tracks` next to `TECHMANIA.exe`
  * This is the root folder of all tracks, and the "Select Track" screen will never go above this folder.
  * For any subfolder in `Tracks`, the game will look for a `track.tech` file in it. If found, the game will treat this folder as a track; otherwise the subfolder itself will show up in "Select Tracks", allowing the player to navigate inside, regardless of content.
  * The content of `track.tech` is an object of type [Track](https://github.com/techmania-team/techmania/blob/master/TECHMANIA/Assets/Scripts/Serializable/Track.cs), serialized as JSON.
  * By default, a track folder is named `<Artist> - <Title> - <creation time>`, but the name does not matter. The game reads in artist and title from `track.tech`, not the folder's name.
* `Skins` next to `TECHMANIA.exe`
  * Contains subfolders corresponding to types of skins. Refer to [Skins](Skins.md) for details.
* `TECHMANIA` in documents folder
  * Stores game options, custom ruleset, and (in the future) records. Refer to [Rulesets](Rulesets.md) for details.
* `C:\Users\<username>\AppData\LocalLow\DJ Hitori\TECHMANIA`
  * The game writes logs to `Player.log` here, which may be useful for troubleshooting.

# Zip archives

When the "Select Track" panel scans the folder shown at the top ("current location"), it will look for and extract all zip archives it sees. There are a few caveats:

* All zip archives will be extracted to the current location.
* All files in the archive that's not inside a folder will be ignored. This is intended to prevent clutter, in case the pattern maker forget to include a folder.
* All empty folders in the archive will be ignored.
* If any error occurred during extraction, the game will log an error (refer to Folders section) and silently move on to the next archive.
* Successfully extracted archives will be deleted.