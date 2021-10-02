Applies to version: 1.4

# On volume and pan

TECHMANIA defines and handles volume and pan differently from whatever game using .pt, so these parameters are not directly convertible. I have experimented and found formulas that I think sound close enough to the original, but you can customize the formulas to your liking if needed.

# Volume conversion

The input is velocity in .pt, ranging from 0 to 127. Output is volume in .tech, ranging from 0 to 1. For convenience we first normalize input, so it ranges from 0 to 1:

`normalized = input / 127`

Then, choose 1 of the 2 following formulas:

* Exponential: `output = normalized ^ exponent`
* Logarithmic: `output = log(base, normalized) + 1` (output is clamped between 0 and 1)

The default is exponential with exponent equal to 1.5.

# Pan conversion

The input is pan in .pt, ranging from 0 to 127. Output is pan in .tech, ranging from -1 to 1. For convenience we first normalize input, so it ranges from 0 to 1:

* If input < 64: `normalized = 1 - input / 64`
* Otherwise: `normalized = (input - 64) / 63`

Then, choose 1 of the 2 following formulas:

* Exponential
  * If input < 64: `output = -normalized ^ exponent`
  * Otherwise: `output = normalized ^ exponent`
* Logarithmic
  * If input < 64: `output = -log(base, normalized) - 1` (output is clamped between -1 and 0)
  * Otherwise: `output = log(base, normalized) + 1` (output is clamped between 0 and 1)

The default is exponential with exponent equal to 0.25.

# Volume notes

.pt has volume notes, which modifies the velocity of all notes after it in the same track. There is no equivalent in TECHMANIA, so by default, the converter applies the same formula to both volume and velocity, then multiplies the results to get the final output volume.

However, if you need to modify patterns after conversion, especially move notes between tracks with different volumes, it may be better to tell the converter to ignore volume notes, in order to treat each track equally. Note that music volume from TECHMANIA's options menu applies to all notes in hidden lanes.

# Visualization

You can use [WolframAlpha](https://www.wolframalpha.com/) to visualize exponential and logarithmic curves. Type into WolframAlpha:

* Exponential: `plot y=x^0.25, 0<=x<=1` (replace 0.25 with your exponential)
* Logarithmic: `plot y=min(max(log(10, x)+1, 0), 1), 0<=x<=1` (replace 10 with your base)

# On scroll speed

Scroll speed determines:
* the converted pattern's BPS (scroll speed 1 = 8 BPS; scroll speed 2 = 4 BPS)
* the formula to use when converting drag note anchors (see [.pt support](pt_support.md))

Therefore it's critical for the converter to find the correct scroll speed for each pattern. The converter will search the following 3 sources for a scroll speed and use the first one found:

* The attribute of the note at track 18, tick 0 (if the "Load scroll speed from track 18" option is turned on and a note if found at this location)
* [Metadata searching](pt_metadata_searching.md)
* The default values specified in the converter options