Applies to version: 1.0

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
  * For any subfolder in `Tracks`, the game will look for a `track.tech` file in it. If found, the game will treat this folder as a track; otherwise the subfolder itself will show up in "Select Track", allowing the player to navigate inside, regardless of content.
  * The content of `track.tech` is an object of type [Track](https://github.com/techmania-team/techmania/blob/master/TECHMANIA/Assets/Scripts/Serializable/Track.cs), serialized as JSON.
  * By default, a track folder is named `<Artist> - <Title> - <creation time>`, but the name does not matter. The game reads in artist and title from `track.tech`, not the folder's name.
  * If the game finds an image file named `eyecatch.jpg` or `eyecatch.png` in a subfolder, that image will show up as the subfolder's eyecatch, instead of the default folder icon.
* `Skins` next to `TECHMANIA.exe`
  * Contains subfolders corresponding to types of skins. Refer to [Skins](Skins.md) for details.
* `TECHMANIA` in documents folder
  * Stores game options (in `options.json`), custom ruleset (in `ruleset.json`), and records (in `records.json`). Refer to [Rulesets](Rulesets.md) for details on the custom ruleset.
* `C:\Users\<username>\AppData\LocalLow\TECHMANIA Team\TECHMANIA`
  * The game writes logs to `Player.log` here, which may be useful for troubleshooting.
  * On 0.9 and before, the logs were at `C:\Users\<username>\AppData\LocalLow\DJ Hitori\TECHMANIA`.

# Streaming assets on mobile

Players on PC can ignore this section.

To simplify the installation process of mobile builds, we include the official tracks and skins in a `StreamingAssets` folder, which is a special folder inside the project. These are automatically copied to an OS-specific location during installation.

The game contains special logic to combine the regular `Tracks` folder and tracks in the streaming assets, so the select track screen shows content from both. The same goes for skins.

Refer to Unity documentation on [Streaming Assets](https://docs.unity3d.com/Manual/StreamingAssets.html).

# Zip archives

When the select track screen scans a folder, it will look for and extract all zip archives it encounters. Please note the following:

* All zip archives will be extracted to the folder they are in.
* All files in the archive that's not inside a folder will be ignored. This is intended to prevent clutter, in case the pattern maker forget to include a folder.
* All empty folders in the archive will be ignored.
* If any error occurred during extraction, the game will log an error (refer to Folders section) and silently move on to the next archive.
* Extracted files will overwrite existing files if they have the same name.
* Successfully extracted archives will be deleted.
