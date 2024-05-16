Applies to version: 2.2

TECHMANIA provides 2 ways to adjust the timing of notes and judgements: note offset and input latency. Calibrate and adjust them from the options menu.

# How timing works

Each note has a "**correct time**", which means the correct time to play this note is exactly this many seconds after the backing track starts. This is also the moment when the scanline will pass the note's exact center.

TECHMANIA runs a timer, which starts when the backing track starts. When the player plays a note, the game asks the timer for the "**current time**". Then, the game compares current time to the note's correct time, and reaches a judgement based on the time difference.

To complicate matters, input devices, especially touchscreens, have input lags, meaning the moment the game receives input is noticeably later than the moment the player made that input.

As an example, say some note's correct time is 2000 ms.

At 1990 ms, the player touches the note on a touchscreen with 50 ms lag.

At 2040 ms, the game receives the touch. The current time is 2040 ms, the correct time is 2000 ms, so the game thinks the player played the note 40 ms too late.

The note's keysound also plays at 2040 ms, when it ideally should play at 1990 ms.

# Note offset

Note offset moves notes ahead or behind in time, by modifying their correct time. [1]

In the example above, say the player sets the note offset to -50 ms. Then, the note's correct time becomes 1950 ms.

At 1940 ms, the player touches the note. At 1990 ms, the game receives the touch, and again thinks the player played the note 40 ms too late.

However, the keysound now plays at 1990 ms, which should sound more in sync with the backing track.

# Input latency

Input latency compensates the calculated time difference without actually moving the notes.

In the example above, say the player sets the input latency to 50 ms. The note's correct time is still 2000 ms.

At 1990 ms, the player touches the note. At 2040 ms, the game receives the touch, calculates the time difference as 40 ms, and then subtracts another 50 ms due to input latency. The final judgement is that the player played the note 10 ms too early, which is closer to what the player actually did.

However, the keysound still plays at 2040 ms.

# Setting both

One recommended way to set note offset and input latency is to make them the opposite of each other.

In the example above, say the player sets the note offset to -50ms, and input latency to 50 ms. Then, the note's correct time becomes 1950 ms.

At 1940 ms, the player touches the note. At 1990 ms, the game receives the touch, calculates the time difference as (40 - 50) = -10 ms, and the keysound plays at 1990 ms.

---

[1] Under the hood this is implemented by running a second timer which is ahead/behind the base timer by the offset amount. Playable notes run on the second timer, while backing track, BGA and hidden notes run on the base timer. However I feel "note offset modifies correct time" is an easier explanation, and in the end they achieve the same effect.
