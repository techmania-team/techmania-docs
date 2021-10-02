Applies to version: 1.4

Here are all the commands, channels and features that are supported. Anything not listed here will be ignored by the converter.

## Metadata
#GENRE, #TITLE, #ARTIST, #BPM, #LNTYPE 1 (#LNTYPE 2 not supported), #LNOBJ

Long notes will be converted to TECHMANIA's hold notes.

## Sound files
#WAVxx, #OGGxx

Alternative extension is supported: if sound.wav is not found, converter will look for sound.ogg, and vice versa.

## BMP files
#BMPxx, but only when it refers to a video in .wmv or .mp4 format

The 1st note (if any) in channel 04 that refers to such a video will become the converted pattern's BGA, all other notes in all BGA channels (04, 06, 07, etc.) are ignored.

## BPM changes
#BPMxx, channel 03, channel 08

## Stops
#STOPxx, channel 09

## Note channels
01, 1x, 2x, 3x, 4x, 5x, 6x

5x and 6x are ignored if #LNTYPE 2.

The channels are mapped to lanes using the following strategy:
* First, for all used channels among 1x-6x, they are mapped to lanes from top to bottom.
* Then, channel 01 and all its repetitions are mapped to the remaining lanes, again from top to bottom.
* If this results in more than 64 lanes, the conversion fails, but this should be rare.
