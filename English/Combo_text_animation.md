Applies to version: 2.3

This page explains how to define an animation for [combo text](Skins.md#combo-skin).

# Animation, curves and key frames

In the context of combo text, an animation is a collection of curves. Each curve controls one attribute of the combo text, and describes how that attribute changes over time.

A curve is a collection of key frames, which specifies the value of the controlled attribute at a specified time.

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
# Loop mode
# Tangent and weight
# A tool to edit curves
