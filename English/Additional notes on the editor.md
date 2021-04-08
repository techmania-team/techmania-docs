Applies to version: 0.4

# File and folder structure
The track folder is located in the same folder as TECHMANIA.exe, named `Tracks`. Each subfolder in `Tracks` corresponds to one track. Each track folder contains all resource files, as well as a `track.tech` file that contains the patterns and metadata. The content of `track.tech` is an object of type [TrackBase](https://github.com/techmania-team/techmania/blob/master/TECHMANIA/Assets/Scripts/Track.cs), serialized as JSON.

By default, a track folder is named `<Artist> - <Title> - <creation time>`, but the name does not matter. The game reads in artist and title from `track.tech`, not the folder's name.

# Supported media formats
* Audio: .wav, .ogg
* Video: .wmv, .mp4
* Image: .jpg, .png

# Other notes

* The preview button in the bottom bar is always grayed out. This is a placeholder for a feature to be added in a future version.
* The editor will always leave at least 1 empty scan at the end of the pattern. If you place a note in the last scan, a new scan will be added. However, removing all notes from the last scan will not delete that scan; close and re-open a pattern to delete excessive empty scans.
