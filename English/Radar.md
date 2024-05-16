Applies to version: 2.2

This page explains how TECHMANIA defines and calculates each value in the radar, seen at the select pattern screen as well as the pattern editor.

# First things first

The radar is meant to help the player form an expectation of the pattern before playing. It is NOT meant to replace the pattern's level chosen by the pattern author.

All values here, including the suggested level, are calculated with hard-coded formulae, and cannot take human creativity into account.

The formulae are calibrated against 4-lane Touch patterns between the levels of 1 and 12, at around 2 minutes in length. They may be less accurate when applied to other patterns.

# Definitions

**Pattern length**: the time duration from the beginning of the first non-empty scan to the end of the last non-empty scan. This calculation does not take end-of-scan into account.

**Raw value**: the value produced by each formula, before applying normalization. These values are only visible in the pattern editor.

**Normalized value**: to better display the radar, TECHMANIA applies a different linear transformation to each raw value so that they fall into the same 0-100 scale. These values are the ones shown on the select pattern screen.

# Density

Describes the overall difficulty of the pattern.

Raw value: `number of notes / pattern length`, in notes per second.

Normalization range: `[0.5, 8]`

This means after normalization, `0.5 notes per second` or less becomes 0, `8 notes per second` or more becomes 100, and values in-between are scaled linearly.

# Peak

Describes the peak difficulty of the pattern.

Raw value: `number of notes / scan length` for the densest scan, in notes per second.

Normalization range: `[1, 18]`

# Speed

Describes the amount of horizontal movement over the pattern.

Raw value: `number of scans / pattern length`, in scans per minute.

Normalization range: `[12, 55]`

# Chaos

Describes how much of the pattern is on unusual timepoints.

A note is a chaos note if it's neither on a whole beat or on a half beat.

Raw value: `number of chaos notes / number of notes`, in percentage.

Normalization range: `[0, 50]`

# Async

Describes how much of the pattern requires the player to touch places not on a scanline.

A note counts as 0.5 async notes if it is a hold note or repeat head hold note; a note counts as 1 async note if it is a repeat note or repeat hold note.

Raw value: `number of async notes / number of notes`, in percentage.

Normalization range: `[0, 40]`

# Suggested level

All values used are raw values. `Round` means round to the nearest integer.

```
Suggested level = Round(
    density * 0.85 +
    peak * 0.12 +
    speed * 0.02 +
    chaos * 0 +
    async * 0.03
    1.12)
```

This formula is calculated using linear regression on 448 patterns. If we apply it to the same patterns and compare the suggested levels with the original levels:
* 45.54% of suggested levels match the original level
* 93.75% of suggested levels are within 1 level from the original level
* 99.33% of suggested levels are within 2 levels from the original level

Due to the relatively low accuracy, the editor will show the suggested level as a range from `Suggested level - 1` to `Suggested level + 1`. If the level you chose is outside of this range, consider adjusting it towards the range.