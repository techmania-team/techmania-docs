Applies to version: 2.3

This page explains how to define an animation for [combo text](Skins.md#combo-skin).

# Animation, curves and key frames

In the context of combo text, an animation is a collection of curves. Each curve controls one attribute of the combo text, and describes how that attribute changes over time.

A curve is a collection of key frames, each specifying the value of the controlled attribute at a specified time.

This is rather abstract so let's look at an example. This is also the default animation if a combo skin does not define an animation.

```
    "animationCurves": [
        {
            "keys": [
                {
                    "time": 0,
                    "value": 1.2
                },
                {
                    "time": 0.1,
                    "value": 0.8
                },
                {
                    "time": 0.133,
                    "value": 1.1
                },
                {
                    "time": 0.167,
                    "value": 1
                },
                {
                    "time": 1,
                    "value": 1
                }
            ],
            "attribute": "scaleX"
        },
        {
            "keys": [
                {
                    "time": 0,
                    "value": 1.2
                },
                {
                    "time": 0.1,
                    "value": 0.8
                },
                {
                    "time": 0.133,
                    "value": 1.1
                },
                {
                    "time": 0.167,
                    "value": 1
                },
                {
                    "time": 0.833,
                    "value": 1
                },
                {
                    "time": 1,
                    "value": 2
                }
            ],
            "attribute": "scaleY"
        },
        {
            "keys": [
                {
                    "time": 0,
                    "value": 0.5,
                    "outTangent": 8
                },
                {
                    "time": 0.1,
                    "value": 1
                },
                {
                    "time": 0.833,
                    "value": 1
                },
                {
                    "time": 1,
                    "value": 0
                }
            ],
            "attribute": "alpha"
        }
    ]
```

This animation consists of 3 curves.
* The first one controls `scaleX`, which stretches the entire combo text in the horizontal direction.
* The second one controls `scaleY`, which is like `scaleX` but in the vertical direction.
* The third one controls `alpha`, which makes the entire combo text transparent.

Looking at the `scaleX` curve, it consists of 5 key frames. Put together, this curve:
* Starts the combo text at 1.2x its normal width when the animation begins (time 0)
* Over the next 0.1 seconds, gradually shrinks the combo text to be 0.8x its normal width
* Over the next 0.033 seconds, gradually stretches again to 1.1x normal width
* Over the next 0.034 seconds, gradually returns the combo text to its normal width
* Holds at normal width until the animation ends

The `scaleY` curve is similar to the `scaleX` one, except there's an additional stretch to 2x normal height near the end.

The `alpha` curve makes the combo text start half transparent, then gradually become fully opaque, hold for a while, then gradually become fully transparent (while `scaleY` curve is doing its final stretch). The `outTangent` thing will be explained later.

The three curves apply simultaneously, combining into one animation for the combo text.

Here's an image representation of the 3 curves:

![image](https://github.com/techmania-team/techmania-docs/assets/30272345/794219cc-b7f0-47de-9d84-2cb0a1323b64)

# animationCurves format

The animation:

```
"animationCurves": [
    <curve 1>,
    <curve 2>,
    ...
]
```

A curve:

```
{
    "keys": [
        <key frame 1>,
        <key frame 2>,
        ...
    ],
    "attribute": <attribute to control>,
    "loopMode": <loop mode>
}
```

`"attribute"` must be one of:
* `"translationX"`
* `"translationY"`
* `"rotationInDegrees"`
* `"scaleX"`
* `"scaleY"`
* `"alpha"`

`"loopMode"` is optional, and must be one of:
* `"once"` (default)
* `"pingpong"`
* `"loop"`

Attributes and loop modes will be explained in later sections.

A key frame:

```
{
    "time": <time>,
    "value": <value>,
    "inTangent": <in tangent>,
    "outTangent": <out tangent>,
    "inWeight": <in weight>,
    "outWeight": <out weight>,
    "weightedMode": <weighted mode>
}
```

All fields other than `"time"` and `"value"` are optional and default to 0. Tangents and weights will be explained in a later section.

# Attributes

Here are all the attributes you can control via curves.

### `translationX` and `translationY`

These attributes move the combo text away from its "normal" position (`distanceToNote` pixels above the note being played). `translationX` moves the combo text left (when negative) and right (when positive); `translationY` moves the combo text down (when negative) and up (when negative).

Unit: pixel (scaled so the game window's height is 1080 pixels)

Default value: 0

![An image showing the effect of translationX and translationY](https://github.com/techmania-team/techmania-docs/assets/30272345/a6c06f34-2baa-4c59-b40c-e707f5389c46)

### `rotationInDegrees`

This attribute rotates the combo text around its center counterclockwise.

Unit: degree

Default value: 0

![An image showing the effect of rotationInDegrees](https://github.com/techmania-team/techmania-docs/assets/30272345/6c23244f-f43e-4a35-bfc8-4e91316580d3)

### `scaleX` and `scaleY`

These attributes stretch or shrink the combo text from its "normal" size, in the horizontal and vertical direction, respectively.

Unit: multiples of the "normal" size

Default value: 1

![An image showing the effect of scaleX and scaleY](https://github.com/techmania-team/techmania-docs/assets/30272345/c434fed8-9677-4af2-a988-25c94b493e26)

### `alpha`

This attribute makes the combo text transparent (0) or opaque (1). Values outside of [0, 1] are meaningless.

Unit: none

Default value: 1

![An image showing the effect of alpha](https://github.com/techmania-team/techmania-docs/assets/30272345/ed645447-6710-4125-90dd-8132d1318fec)

# Loop mode

Each curve may optionally specify a loop mode, which indicates how the animation behaves when time is outside of the range defined by key frames.

### `once`

The animation will play once and hold at the value at the final key frame. This is the default loop mode.

### `pingpong`

The animation will play to the end, then reverse to the beginning as if time flows backwards, then play forward to the end again, ad infinitum.

### `loop`

The animation will play to the end, then instantly revert to the beginning and play again to the end, ad infinitum.

![An image showing the three loop modes](https://github.com/techmania-team/techmania-docs/assets/30272345/1428d635-06ee-4d35-bbb4-7b9be20ec5cc)

# Tangent and weight

If you need finer control of your curve's shape, tangent and weight will come into play. They are defined on key frames, and control how a curve's value is interpolated between each pair of key frames.

### `inTangent` and `outTangent`

`inTangent` controls the slope of the curve as it approaches the key frame from the previous one; `outTangent` controls the slope of the curve as it leaves the key frame towards the next one. Tangents can be any real number, and the default value is zero.

![An image showing the curve shape at different tangents](https://github.com/techmania-team/techmania-docs/assets/30272345/1fd6323e-16bc-4d9a-befc-042e4b59f2ae)

### `inWeight`, `outWeight` and `weightedMode`

By default, each segment of the curve between two key frames is affected equally by their values and tangents, but this can be changed via weight.

Before setting `inWeight` and `outWeight`, make sure you set `weightedMode` to a non-zero value so they are meaningful. The possible values of `weightedMode` are:

* 0: both `inWeight` and `outWeight` are meaningless
* 1: `inWeight` affects the curve segment before the key frame; `outWeight` is meaningless
* 2: `outWeight` affects the curve segment after the key frame; `inWeight` is meaningless
* 3: `inWeight` and `outWeight` affect the curve segments before and after the key frame, respectively

When `inWeight` and `outWeight` are meaningful, their values should be in [0, 1], and the default value is 0. Larger values mean the key frame's value and tangent have a larger influence on the curve's shape before or after the key frame.

When `inWeight` and `outWeight` are meaningless, the curve will be calculated as if the weight is 0.333.

![An image showing the curve shape at different tangents](https://github.com/techmania-team/techmania-docs/assets/30272345/ad43a77a-09b4-4183-807d-d59d91897ee7)

# A tool to edit curves

To make it easier to define curves, we provide a script that allows you to use Unity's built-in tools to edit a curve, and translate it to the combo skin's format. To use this script:

* Download and install [Unity Hub](https://unity.com/unity-hub).
* In Unity Hub, download and install any recent Unity version.
    * The script and following instructions are written in Unity 2022.3.36f1, but I don't expect them to break in future versions.
* Still in Unity Hub, create a new project, using the "2D (Built-In Render Pipeline)" template. Feel free to turn off "Connect to Unity Cloud".

![A screenshot of creating a new project in Unity Hub](https://github.com/techmania-team/techmania-docs/assets/30272345/a6815ff4-b4fb-4c75-afef-5f82e3432b8b)

* Now in Unity, look at the Project tab, navigate to the "Assets" folder if not already in it. Right click - Create - C# Script, then name the new script `CurveTest`.

![A screenshot of creating a new script](https://github.com/techmania-team/techmania-docs/assets/30272345/1dcd62f3-5c2e-4285-bd57-e33c0e7e008c)

* Double click `CurveTest` to open it in a text editor, which may be Visual Studio that you installed alongside Unity. Delete all existing content in the file, and paste in the following:

```C#
using System.Text;
using UnityEngine;

public class CurveTest : MonoBehaviour
{
    public AnimationCurve curve;

    private void OnGUI()
    {
        StringBuilder stringBuilder = new StringBuilder();
        stringBuilder.AppendLine("{");
        stringBuilder.AppendLine("  \"keys\": [");
        for (int i = 0; i < curve.keys.Length; i++)
        {
            Keyframe k = curve.keys[i];
            stringBuilder.AppendLine("    {");
            stringBuilder.AppendLine($"      \"time\": {k.time},");
            stringBuilder.AppendLine($"      \"value\": {k.value},");
            stringBuilder.AppendLine($"      \"inTangent\": {k.inTangent},");
            stringBuilder.AppendLine($"      \"outTangent\": {k.outTangent},");
            stringBuilder.AppendLine($"      \"inWeight\": {k.inWeight},");
            stringBuilder.AppendLine($"      \"outWeight\": {k.outWeight},");
            stringBuilder.AppendLine($"      \"weightedMode\": {(int)k.weightedMode}");
            stringBuilder.Append("    }");
            if (i < curve.keys.Length - 1)
            {
                stringBuilder.Append(",");
            }
            stringBuilder.AppendLine();
        }
        stringBuilder.AppendLine("  ],");
        stringBuilder.AppendLine("  \"attribute\": \"REPLACE WITH THE ATTRIBUTE TO ANIMATE\"");
        stringBuilder.AppendLine("}");
        GUI.Window(0, new Rect(0, 0, Screen.width, Screen.height),
            (int windowID) =>
            {
                GUI.TextArea(new Rect(0, 20, Screen.width, Screen.height - 20),
                    stringBuilder.ToString());
            }, "");
    }
}
```

* Save and return to Unity, then drag the `CurveTest` script to the `Main Camera` game object in the Hierarchy tab. This adds `CurveTest` as a component to `Main Camera`.

![A screenshot showing where the drag should start and end](https://github.com/techmania-team/techmania-docs/assets/30272345/11bb5d21-b0f6-460d-b0be-b01c09296d35)

* Click the `Main Camera` game object to select it. In the Inspector tab, scroll to the bottom to find the `CurveTest` component, which should have a property called `Curve`. Click the rectangle right to it to open Unity's curve editor. You can now begin editing your curve.

![A screenshot showing where to find the curve property](https://github.com/techmania-team/techmania-docs/assets/30272345/9f43e225-1c59-4e8f-9bfe-d6843cee6695)

* At any time you can click the Play button to enter play mode. In the Game view, you will see a text representation of the curve you made, which you can copy and paste into your combo skin. You can continue to edit your curve and the text will update to reflect your changes.
    * Remember that the combo text animation is an array of curves, you need to write [] outside all curves.

![A screenshot showing the play button and the game view](https://github.com/techmania-team/techmania-docs/assets/30272345/f091bc05-2514-414f-a4bd-55811b978b6b)

