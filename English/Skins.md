Applies to version: 0.8

This page explains how to make skins for TECHMANIA.

# Types and locations

There are currently 4 types of skins in TECHMANIA:
* Note skin: determines the appearance of notes, located at `<TECHMANIA folder>/Skins/Note/<skin name>`.
* VFX skin: determines the appearance of VFX, including the explosion effects when notes are played, and the effect overlaid on notes when Fever is active. Located at `<TECHMANIA folder>/Skins/VFX/<skin name>`.
* Combo skin: determines the appearance of combo text, which is the line of text that appears above played notes to tell you the judgement and current combo. Located at `<TECHMANIA folder>/Skins/Combo/<skin name>`.
* Game UI skin: determines the appearance of scanlines, scan countdown, touch/click feedback, and approach overlay. Located at `<TECHMANIA folder>/Skins/Game UI/<skin name>`.

Each skin is a folder containing a number of sprite sheets and a `skin.json` file, both of which will be explained later.

# Sprite sheets

All skins are based on sprite sheets so it's important to understand them. The term may have different meanings in other fields, but in the context of TECHMANIA skins, a sprite sheet is a series of sprites, tiled horizontally into one image, where each sprite corresponds to one frame of an animation. The sprites must be identical in size, and tiled tightly, with optional paddings inside each tile, but without any space between tiles.

![An image showing a sprite sheet, a tile, a sprite, and padding](https://imgur.com/hiuagYc.png)

(Example sprite sheets in this page are taken from [https://www.gamedesigning.org/animation/sprites/](https://www.gamedesigning.org/animation/sprites/))

Not all tiles in a sprite sheet must contain a sprite. It's okay to leave a few empty at the beginning and/or end of the sprite sheet.

![A sprite sheet with empty tiles at the beginning and end](https://imgur.com/QY62oea.png)

For each sprite sheet, you need to write an accompanying JSON object to describe it to TECHMANIA, which looks like:
```
{
  "filename": <filename>,
  "rows": <rows of tiles>,
  "columns": <columns of tiles>,
  "firstIndex": <index of the first tile that contains a sprite>,
  "lastIndex": <index of the last tile that contains a sprite>,
  "padding": <amount of padding in pixels>,
  "bilinearFilter": <true or false>,
  
  "scale": <number, skin-specific>,
  "speed": <number, skin-specific>,
  "additiveShader": <true or false, skin-specific>
}
```

Tiles are indexed in row-major order, and start from 0. For example, the JSON object that describes the 2nd sprite sheet above may look like:
```
{
  "filename": "sprite sheet.png",
  "rows": 3,
  "columns": 4,
  "firstIndex": 1,
  "lastIndex": 10
}
```
In this example, the first row contains tiles #0, #1, #2 and #3, the second row contains #4, #5, #6 and #7, and the third row contains #8, #9, #10 and #11. The tiles containing sprites are #1 to #10.

## Artifacts, bilinear filter and padding

By default, TECHMANIA applies bilinear filter when scaling sprites to different sizes. This makes the scaled image smoother, but may produce artifacts at the edges when sprites are not transparent at all 4 edges, because the filter will read pixels from slightly outside the sprites' borders, or in other words, from neighboring sprites. There are 2 ways to remove artifacts:

* Turn bilinear filter off by setting `"bilinearFilter": false` in the sprite sheet. This prevents artifacts by forcing the game to read whole pixels instead of interpolating between neighboring pixels; as a side effect, this also causes scaled sprites to appear pixelated.
* Add padding to each sprite. The amount of padding must be uniform in all 4 directions. This prevents artifacts by ensuring the neighboring pixels are transparent; as a side effect, this also causes sprites to appear transparent around the edges.
  * You can use [this Photoshop script](https://gist.github.com/macmillan333/daa66be5ee4bc14f69b2ad95867f1270) to add padding to existing sprite sheets.

It is recommended that you try padding first, and if the result looks weird, fall back to turning off bilinear filter.
![An image showing the same sprite sheet scaled with bilinear filter on and off, and producing an artifact when on](https://imgur.com/n6nWt55.png)

## Default values

All fields other than `filename` are optional, and will take the following default values if you omit them:
* `rows`: 1
* `columns`: 1
* `firstIndex`: 0
* `lastIndex`: 0
* `padding`: 0
* `bilinearFilter`: true
* `scale`: 1
* `speed`: 1
* `additiveShader`: false

This means if you write a filename and nothing else, TECHMANIA will load the entire file as one sprite.

## Skin-specific fields

`scale`, `speed` and `additiveShader` are only supported by specific items in specific skins. In the skins that support them:

* `scale` determines the in-game size of sprites, in multiples of a lane's height. Different skins define this parameter slightly differently, which will be discussed in detail in their corresponding sections.
* `speed` determines the speed of the animaion, in multiples of 60 frames per second. For example, `"speed": 0.5` means the animation plays at 30 frames per second.
* `additiveShader` determines whether the sprites are rendered with an additive shader. This will cause the colors of the sprites to be added to the layers below them, instead of replace the layers below them.

![An image demonstrating normal and additive shaders](https://imgur.com/oR6gHqA.png)

# Note skin
The `skin.json` file in a note skin follows the following format:
```
{
  "version": "1",
  "author": <author>,
  "basic": <basic note>,
  "chainHead": <chain head>,
  "chainNode": <chain node>,
  "chainPath": <chain path>,
  "dragHead": <drag head>,
  "dragCurve": <drag curve>,
  "holdHead": <hold head>,
  "holdTrail": <hold trail>,
  "holdTrailEnd": <hold trail end>,
  "holdOngoingTrail": <hold ongoing trail>,
  "repeatHead": <repeat head>,
  "repeat": <repeat>,
  "repeatHoldTrail": <repeat hold trail>,
  "repeatHoldTrailEnd": <repeat hold trail end>,
  "repeatPath": <repeat path>
}
```
Each value is a sprite sheet. Refer to the [Terminology](Terminology.md) page for the meaning of most fields.

Most sprite sheets in a note skin contain an additional field called `scale`:
* For `basic`, `chainHead`, `chainNode`, `dragHead`, `holdHead`, `repeatHead` and `repeat`, the sprites will be scaled to a square, whose side length is equal to `scale * lane height`. This is only visual, and has no effect on hitbox sizes.
* For `chainPath`, `dragCurve`, `holdTrail`, `holdOngoingTrail`, `repeatHoldTrail` and `repeatPath`, the game will stretch the sprites in one dimension (horizontal, towards other notes, or in the curve's direction) according to the pattern, so the sprite's size in that dimension is unaffected by `scale`. The size in the perpendicular dimension will be scaled to `scale * lane height`.
* `holdTrailEnd` and `repeatHoldTrailEnd` do not contain a `scale` field.
* If omitted, `scale` takes the default value of 1.

![An image demonstrating the scale of repeat head and repeat hold trail](https://imgur.com/m1EO3jo.png)

`dragCurve` sprites are stretched in a special way: the left half is stretched to form the curve's body, and the right half is left intact to form the curve's end.

![An image demonstrating a drag curve being stretched in the left half](https://imgur.com/1wZbgA6.png)

`holdTrailEnd` and `repeatHoldTrailEnd` are images attached to the end of their respective hold trails, typically used for shadows and glows. They will be automatically scaled so that their height match the hold trail, while keeping the aspect ratio of the sprites unchanged.

![An image demonstrating a repeat hold trail and its end](https://imgur.com/lNSPcJk.png)

`holdOngoingTrail` is the trail drawn over the portion of a hold note that the scanline has passed.

![An image demonstrating a hold ongoing trail](https://imgur.com/6Vib3rT.png)

All animations will play at a speed of one cycle per beat, and the 1st sprites in each sprite sheet will be the ones shown at whole beats.

For the sprites in `basic`, `chainHead`, `chainNode`, `dragHead`, `holdHead`, `repeatHead` and `repeat`, TECHMANIA will not flip them regardless of scan direction. Furthermore, for `chainHead`, `chainNode` and `dragHead`, TECHMANIA assumes the sprites point to the right, and from there will rotate them to point in the direction of the next chain node or the curve. The last node in a chain will point to the same direction as the second last node.

# VFX skin
The `skin.json` file in a VFX skin follows the following format:
```
{
  "version": "1",
  "author": <author>,
  "feverOverlay": <fever overlay>,
  "basicMax": [<layer 0>, <layer 1>, ..., <layer n>],
  "basicCool": [<layer 0>, <layer 1>, ..., <layer n>],
  "basicGood": [<layer 0>, <layer 1>, ..., <layer n>],
  "dragOngoing": [<layer 0>, <layer 1>, ..., <layer n>],
  "dragComplete": [<layer 0>, <layer 1>, ..., <layer n>],
  "holdOngoingHead": [<layer 0>, <layer 1>, ..., <layer n>],
  "holdOngoingTrail": [<layer 0>, <layer 1>, ..., <layer n>],
  "holdComplete": [<layer 0>, <layer 1>, ..., <layer n>],
  "repeatHead": [<layer 0>, <layer 1>, ..., <layer n>],
  "repeatNote": [<layer 0>, <layer 1>, ..., <layer n>],
  "repeatHoldOngoingHead": [<layer 0>, <layer 1>, ..., <layer n>],
  "repeatHoldOngoingTrail": [<layer 0>, <layer 1>, ..., <layer n>],
  "repeatHoldComplete": [<layer 0>, <layer 1>, ..., <layer n>]
}
```

In a VFX skin, each effect (except for Fever overlay) is drawn in multiple layers, each layer being one sprite sheet. The layers will be drawn in the order of their definition, so the last layer in the list will show up at the top. You cannot omit the `[]` even when there is only 0 or 1 layers in an effect.

Each sprite sheet in a VFX skin contains 3 additional fields:
* `scale`, which determines the size of the VFX. All sprites will be scaled so that their heights are equal to `scale * lane height`, while keeping the aspect ratios unchanged. If omitted, default value is 1.
* `speed`, which determines the speed of the animations, in multiples of 60 frames per second. For example, `"speed": 0.5` means a VFX will play at 30 frames per second. If omitted, default value is 1.
* `additiveShader`, which tells TECHMANIA whether to draw the sprites with an additive shader. This will cause the colors of a layer to be added to the layers below it, instead of replace the layers below it. If omitted, default value is `false`.

An explanation of what each effect is, and whether the animations loop:
* `feverOverlay`: a looping effect drawn on certain notes when Fever is active.
* `basicMax` / `basicCool` / `basicGood`: one-shot effects drawn when basic notes or chain notes are played with MAX / COOL / GOOD judgements.
* `dragOngoing`: a looping effect drawn at the note head while a drag note is being played.
* `dragComplete`: a one-shot effect drawn at the end of the curve when a drag note is completed.
* `holdOngoingHead` / `holdOngoingTrail`: looping effects drawn at the note head / scanline while a hold note is being played.
* `holdComplete`: a one-shot effect drawn at the end of the trail when a hold note is completed.
* `repeatHead` / `repeatNote`: one-shot effects drawn at the repeat head / repeat note when a repeat note is played.
* `repeatHoldOngoingHead` / `repeatHoldOngoingTrail`: looping effects drawn at the note head / scanline while a repeat hold note is being played.
* `repeatHoldComplete`: a one-shot effect drawn at the end of the trail when a repeat hold note is completed.

# Combo skin
The `skin.json` file in a combo skin follows the following format:
```
{
  "version": "1",
  "author": <author>,
  "distanceToNote": <distance to note>,
  "height": <height>,
  "spaceBetweenJudgementAndCombo": <space between judgement and combo>,
  "feverMaxJudgement": <Fever MAX judgement>,
  "rainbowMaxJudgement": <Rainbow MAX judgement>,
  "maxJudgement": <MAX judgement>,
  "coolJudgement": <COOL judgement>,
  "goodJudgement": <GOOD judgement>,
  "missJudgement": <MISS judgement>,
  "breakJudgement": <BREAK judgement>,
  "feverMaxDigits": [<digit 0>, <digit 1>, ..., <digit 9>],
  "rainbowMaxDigits": [<digit 0>, <digit 1>, ..., <digit 9>],
  "maxDigits": [<digit 0>, <digit 1>, ..., <digit 9>],
  "coolDigits": [<digit 0>, <digit 1>, ..., <digit 9>],
  "goodDigits": [<digit 0>, <digit 1>, ..., <digit 9>] 
}
```

Each judgement and digit is a sprite sheet. Each sprite sheet in a combo skin contains an additional field called `speed`, which has the same meaning as in VFX skin.

The combo text contains 3 parts: the judgement, some horizontal space, and up to 4 digits that show the current combo. If the judgement is MISS or BREAK, the space and digits are not shown; otherwise, the digits are taken from the list that matches the judgement.

The following diagram shows the meaning of `distanceToNote`, `height` and `spaceBetweenJudgementAndCombo`:

![An image showing the meaning of distanceToNote, height and spaceBetweenJudgementAndCombo](https://imgur.com/J3VP1qF.png)

All 3 numbers are in integer pixels. As a reference, the game window's height is always 1080 pixels regardless of resolution. All sprites in the combo text will be scaled so that their heights are equal to `height`, while keeping their aspect ratios unchanged.

While each judgement and combo digit may play animations of their own, the game also plays a built-in animation on the entire combo text as a whole, including a fade-in, a fade-out and scaling. Currently this animation cannot be customized.

# Reloading skins

By default, TECHMANIA only loads skins when:
* Starting up
* The player changes skins in the select skin menu

In the select skin menu, there's an option to also reload the skins each time you load a pattern. This may be useful for making and testing new skins, but it increases load time, so remember to turn it off after you complete your skin.

# JSON tips

Keep the following in mind when writing JSON:
* All field names require quotation marks.
* Values only require quotation marks if they are strings (for skins, only the `filename` fields are strings).
* If there are multiple fields and values between a pair of `[]` or `{}`, write commas after each value except for the last one.
* Consider using a text editor meant for programming, such as Visual Studio Code, because they can spot and warn you about JSON format errors.
