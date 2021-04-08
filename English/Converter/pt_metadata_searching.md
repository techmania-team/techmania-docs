Applies to version: 1.2

When converting from .pt, the converter will attempt to search various locations for pieces of metadata. All of the following except for song name are optional.

In the following sections, `pt folder` refers to the input folder containing .pt files, and `..` means go up one level. For example, `C:\A\B\..\C.txt` refers to `C:\A\C.txt`.

# Song name

The converter expects to find files named `<song name>_star_<x>.pt` or `<song name>_pop_<x>.pt` in `pt folder`, where `<song name>` is any string, and `<x>` is 1, 2, 3 or 4. If no such files are found, the conversion cannot start.

After this step, the converter treats `<song name>` as an identifier of the song. It will become the converted track's title if the following search for a title fails.

# Title, genre, composer, difficulty

The converter will attempt to open `pt folder\..\..\Resource\DiscInfo\discstock.csv`. If the file exists, the converter will search for a row matching `<song name>`, and extract from it the title, genre, composer and difficulty of each pattern.

# Scroll speed

The converter will attempt to open `pt folder\..\..\Resource\DiscInfo\<mode>_stage_<x>.csv`, where `<mode>` is `pop` or `star`, and `<x>` is 1, 2, 3 or `bonus`. If these files exist, the converter will search for a row matching `<song name>`, and extract from it the scroll speed of each pattern.

Scroll speed becomes the converted pattern's BPS. Scroll speed 1 means 8 beats per scan; scroll speed 2 means 4 beats per scan. If scroll speed is not found, the converter will assume 4 beats per scan.

# Disc image

The converter will look for a disc image in the following order:
* `pt folder\..\..\Resource\Discimg\<song name>_<x>.png`, where `<x>` is any character
* `pt folder\<song name>_disc.png`

The first file found (if any) becomes the converted track's eyecatch image.

# Eyecatch

The converter will look for an eyecatch image in the following order:
* `pt folder\..\..\Resource\Eyecatch\Song\<song name>_<x>.jpg`, where `<x>` is any character
* `pt folder\<song name>_eyecatch.jpg`

The first file found (if any) becomes the converted track's background image.

# Preview

The converter will look for a preview track in the following order:
* `pt folder\..\..\Resource\Preview\<song name>.ogg`
* `pt folder\..\..\Resource\Previewogg\<song name>.ogg`
* `pt folder\<song name>_pre.ogg`

The first file found (if any) becomes the converted track's preview track.

# BGA

The converter will look for a BGA in the following order:
* `pt folder\..\..\Movie\<song name>.bik`
* `pt folder\..\..\Movie<x>\<song name>.bik`, where `<x>` is any number between 1 and 20 (inclusive)
* `pt folder\<song name>.mp4`

The first file found (if any) becomes the BGA of all converted patterns. The converter will launch ffmpeg to convert .bik to .mp4 if necessary.
