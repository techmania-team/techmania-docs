Applies to version: 2.2

Due to the variety of note types, it is possible to place notes in a way that may cause the game to freeze when loading, make some notes appear weird, make some notes unplayable, or make the pattern hard to read. In an effort to reduce these issues, we establish the following rules on patterns. Most of these do not apply to hidden notes.

The editor will attempt to enforce most of these rules, but ultimately it is up to the pattern author to fully avoid violations.

Please refer to the [Terminology](Terminology.md) page when necessary.

# Universal: all notes
* ◆ Notes can not be placed at the exact same (pulse, lane) with each other.
* ◆ Notes can not be placed before pulse 0.
* ◆ Notes can not be placed above lane 0 or below lane 63.

# Universal: long notes (drag, hold, repeat hold)
* ◆ Long notes can not cover other notes. [1]

# Chain notes
* ◇ Each chain head must be connected to at least 1 chain node.
* ◇ Each chain node must be connected to a chain head.
* ◆ No 2 chain heads or chain nodes can be placed at the same pulse. This includes hidden notes.
* ◇ Each chain node must be placed in an adjacent lane to the chain head or chain node before it.
* ◇ Chain paths can not cross scans.
* Chain paths can not cover other notes.

# Drag notes
* ◇ Drag curves can not cross scans.
* ◇ If a drag curve starts within the playable lanes, it can not ever go above or below the playable lanes.
* ◆ The anchors and control points can not be set in a way that makes the curve flow to the left at any point.
* ◆ A drag note must contain at least 2 anchors.

# Hold notes
* ◇ Hold notes can not be longer than 2 scans.

# Repeat notes
* ◇ Each repeat head must be connected to at least 1 repeat note.
* ◇ Each repeat note (including repeat hold note) must be connected to a repeat head.
* ◇ Repeat notes, or the end of repeat hold notes, can not be more than 2 scans away from the repeat head it's connected to. Note that crossing scans is allowed, but discouraged.
* ◇ Repeat paths can not cover other notes.

# Interaction between different types
* Chain paths, repeat paths, hold trails, repeat hold trails and drag curves can not cross each other.

# Control-scheme-specific
* ◇ In Keys, drag notes can not cross lanes.
* ◇ In KM, no 2 basic notes, chain heads, chain nodes or drag heads can be placed at the same pulse.
* ◇ In KM, no basic notes, chain heads, chain nodes or drag heads can be placed at a pulse where a drag curve is present.

# Legend

◆ - the editor will stop you from making an edit that violates this rule.

◇ - the editor will spot violations to this rule when you click the Inspect button.

No mark - the editor is unable to enforce this rule.

---

[1] not enforced on drag notes due to their complexity.