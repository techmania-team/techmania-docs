Applies to version: 1.4

When converting from .pt, the converter will attempt to search various locations for pieces of metadata. All of the following except for song name are optional.

In the following sections, `pt folder` refers to the input folder containing .pt files, and `..` means go up one level. For example, `C:\A\B\..\C.txt` refers to `C:\A\C.txt`.

# Song name

The converter expects to find files named `<song name>_star_<x>.pt` or `<song name>_pop_<x>.pt` in `pt folder`, where `<song name>` is any string, and `<x>` is 1, 2, 3 or 4. If no such files are found, the conversion cannot start.

After this step, the converter treats `<song name>` as an identifier of the song. It will become the converted track's title if the following search for a title fails.

# Title, genre, composer, difficulty

The converter will attempt to open `pt folder\..\..\Resource\DiscInfo\discstock.csv`. If the file exists, the converter will search for a row matching `<song name>`, and extract from it the title, genre, composer and difficulty of each pattern.

# Scroll speed

The converter will attempt to open `pt folder\..\..\Resource\DiscInfo\<mode>_stage_<x>.csv`, where `<mode>` is `pop` or `star`, and `<x>` is 1, 2, 3 or `bonus`. If these files exist, the converter will search for a row matching `<song name>`, and extract from it the scroll speed of each pattern.

This is only 1 of 3 possible sources for scroll speed. See [.pt converter options](pt_converter_options.md) for more details.

# Disc image

The converter will look for a disc image in the following order:
* `pt folder\..\..\Resource\Discimg\<song name>_<x>.<extension>`, where `<x>` is `0`, `1`, `2` or `3`, and `<extension>` is `png` or `jpg`
* `pt folder\<song name>_disc.<extension>`, where `<extension>` is `png` or `jpg`

The first file found (if any) becomes the converted track's eyecatch image.

# Eyecatch

The converter will look for an eyecatch image in the following order:
* `pt folder\..\..\Resource\Eyecatch\Song\<song name>_<x>.<extension>`, where `<x>` is `0`, `1`, `2` or `3`, and `<extension>` is `png` or `jpg`
* `pt folder\<song name>_eyecatch.<extension>`, where `<extension>` is `png` or `jpg`

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

# Song script

The converter will attempt to open `pt folder\..\..\Resource\Script\Song\<song name>\<song name>_<mode>_<x>.ini`, where `<mode>` is `pop` or `star`, and `<x>` is 1, 2, 3 or 4. If the file exists, the converter will convert the data within to a legacy ruleset override in the corresponding pattern (1 for NM, 2 for HD, 3 for MX, 4 for EX). The conversion consists of the following steps:

* Open `pt folder\..\..\Resource\Script\Maingame\JudgmentTempInfo.xml`, read time windows from the `<Delta>` element inside the `<PopMode>` element, and read HP deltas from the `<Gauge>` elements inside the `<PopMode>` element.
  * Since time windows are the same across all note types, the converter only reads the windows for "Normal" notes.
  * The "Fair" time window is skipped as it's unused.
* In the `.ini` file, read time windows from the `[JudgmentDelta]` section, and read HP deltas from the `[GaugeUpDownRate]` section.
  * The `JUDGMENT_FAIL` time window is skipped as it's unused.
* Add the time windows and HP deltas from the previous 2 steps.
* Multiply time windows by 1.25; multiply HP deltas by 100.