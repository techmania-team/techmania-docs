Applies to version: 2.2

A ruleset is a collection of parameters that determine the difficulty of TECHMANIA. The game contains the following rulesets:
- Standard: the default, intended ruleset
- Legacy: a stricter ruleset inspired by a relic from the past
- Custom: a fully customizable ruleset

You can switch ruleset from the options menu. Please note that records are tied to rulesets, so a record set using the standard ruleset will not appear when using the legacy ruleset, and vice versa. No record will be set when using the custom ruleset.

# How to define custom ruleset

1. Make sure you have launched TECHMANIA and visited the options menu at least once.
2. In File Explorer, look for a folder called `TECHMANIA` in your documents folder. It's usually located at `C:\Users\<username>\OneDrive\Documents\TECHMANIA` or `C:\Users\<username>\Documents\TECHMANIA`. You should see a file called `options.json` here.
3. Next to `options.json`, create a new file called `ruleset.json`.
4. Open `ruleset.json` with any text editor, paste in one of the two following rulesets as a starting point, and save:

Standard ruleset:

```
{
    "version": "3",

    "windowsAndDeltas": {
        "timeWindows": [0.04, 0.07, 0.1, 0.15, 0.2],
        "hpDeltaBasic": [3, 3, 3, 3, -50, -50],
        "hpDeltaChain": [3, 3, 3, 3, -50, -50],
        "hpDeltaHold": [3, 3, 3, 3, -50, -50],
        "hpDeltaDrag": [3, 3, 3, 3, -50, -50],
        "hpDeltaRepeat": [3, 3, 3, 3, -50, -50],
        "hpDeltaBasicDuringFever": [5, 5, 5, 5, -50, -50],
        "hpDeltaChainDuringFever": [5, 5, 5, 5, -50, -50],
        "hpDeltaHoldDuringFever": [5, 5, 5, 5, -50, -50],
        "hpDeltaDragDuringFever": [5, 5, 5, 5, -50, -50],
        "hpDeltaRepeatDuringFever": [5, 5, 5, 5, -50, -50]
    },
    "windowsAndDeltasSetlist": [
        {
            "timeWindows": [0.04, 0.07, 0.1, 0.15, 0.2],
            "hpDeltaBasic": [2, 2, 2, 2, -30, -30],
            "hpDeltaChain": [2, 2, 2, 2, -30, -30],
            "hpDeltaHold": [2, 2, 2, 2, -30, -30],
            "hpDeltaDrag": [2, 2, 2, 2, -30, -30],
            "hpDeltaRepeat": [2, 2, 2, 2, -30, -30],
            "hpDeltaBasicDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaChainDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaHoldDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaDragDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaRepeatDuringFever": [3, 3, 3, 3, -30, -30]
        },
        {
            "timeWindows": [0.04, 0.07, 0.1, 0.15, 0.2],
            "hpDeltaBasic": [2, 2, 2, 2, -30, -30],
            "hpDeltaChain": [2, 2, 2, 2, -30, -30],
            "hpDeltaHold": [2, 2, 2, 2, -30, -30],
            "hpDeltaDrag": [2, 2, 2, 2, -30, -30],
            "hpDeltaRepeat": [2, 2, 2, 2, -30, -30],
            "hpDeltaBasicDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaChainDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaHoldDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaDragDuringFever": [3, 3, 3, 3, -30, -30],
            "hpDeltaRepeatDuringFever": [3, 3, 3, 3, -30, -30]
        },
        {
            "timeWindows": [0.04, 0.07, 0.1, 0.15, 0.2],
            "hpDeltaBasic": [1, 1, 1, 1, -30, -30],
            "hpDeltaChain": [1, 1, 1, 1, -30, -30],
            "hpDeltaHold": [1, 1, 1, 1, -30, -30],
            "hpDeltaDrag": [1, 1, 1, 1, -30, -30],
            "hpDeltaRepeat": [1, 1, 1, 1, -30, -30],
            "hpDeltaBasicDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaChainDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaHoldDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaDragDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaRepeatDuringFever": [2, 2, 2, 2, -30, -30]
        },
        {
            "timeWindows": [0.04, 0.07, 0.1, 0.15, 0.2],
            "hpDeltaBasic": [1, 1, 1, 1, -30, -30],
            "hpDeltaChain": [1, 1, 1, 1, -30, -30],
            "hpDeltaHold": [1, 1, 1, 1, -30, -30],
            "hpDeltaDrag": [1, 1, 1, 1, -30, -30],
            "hpDeltaRepeat": [1, 1, 1, 1, -30, -30],
            "hpDeltaBasicDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaChainDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaHoldDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaDragDuringFever": [2, 2, 2, 2, -30, -30],
            "hpDeltaRepeatDuringFever": [2, 2, 2, 2, -30, -30]
        }
    ],
    
    "timeWindowsInPulses": false,
    "longNoteGracePeriod": 0.15,
    "longNoteGracePeriodInPulses": false,

    "scanMarginTopBottom": [0.05, 0.05, 0.05],
    "scanMarginMiddle": [0.05, 0.05, 0.05],
    "scanMarginBeforeFirstBeat": 0.15,
    "scanMarginAfterLastBeat": 0.1,
    "hitboxWidth": 1.5,
    "hitboxHeight": 1,
    "chainHeadHitboxWidth": 1.5,
    "chainNodeHitboxWidth": 3,
    "dragHitboxWidth": 1.5,
    "dragHitboxHeight": 1,
    "ongoingDragHitboxWidth": 2,
    "ongoingDragHitboxHeight": 2,

    "maxHp": 1000,

    "comboBonus": false,

    "constantFeverCoefficient": false,
    "feverBonusOnMax": 1,
    "feverBonusOnCool": 1,
    "feverBonusOnGood": 0
}
```

Legacy ruleset:

```
{
    "version": "3",

    "windowsAndDeltas": {
        "timeWindows": [12.5, 37.5, 51.25, 65, 83.75],
        "hpDeltaBasic": [30, 30, 15, 0, -300, -600],
        "hpDeltaChain": [30, 30, 15, 0, -350, -500],
        "hpDeltaHold": [30, 30, 15, 0, -350, -500],
        "hpDeltaDrag": [30, 30, 15, 0, -350, -500],
        "hpDeltaRepeat": [30, 30, 15, 0, -350, -500],
        "hpDeltaBasicDuringFever": [30, 30, 30, 0, -300, -600],
        "hpDeltaChainDuringFever": [30, 30, 30, 0, -350, -500],
        "hpDeltaHoldDuringFever": [30, 30, 30, 0, -350, -500],
        "hpDeltaDragDuringFever": [30, 30, 30, 0, -350, -500],
        "hpDeltaRepeatDuringFever": [30, 30, 30, 0, -350, -500]
    },
    "windowsAndDeltasSetlist": [
        {
            "timeWindows": [12.5, 37.5, 51.25, 65, 83.75],
            "hpDeltaBasic": [15, 15, 0, 0, -200, -300],
            "hpDeltaChain": [15, 15, 0, 0, -200, -300],
            "hpDeltaHold": [7, 7, 0, 0, -200, -380],
            "hpDeltaDrag": [7, 7, 0, 0, -200, -380],
            "hpDeltaRepeat": [15, 15, 0, 0, -200, -300],
            "hpDeltaBasicDuringFever": [15, 15, 15, 0, -200, -300],
            "hpDeltaChainDuringFever": [15, 15, 15, 0, -200, -300],
            "hpDeltaHoldDuringFever": [7, 7, 7, 0, -200, -380],
            "hpDeltaDragDuringFever": [7, 7, 7, 0, -200, -380],
            "hpDeltaRepeatDuringFever": [15, 15, 15, 0, -200, -300]
        },
        {
            "timeWindows": [12.5, 37.5, 51.25, 65, 83.75],
            "hpDeltaBasic": [10, 10, 0, 0, -200, -400],
            "hpDeltaChain": [10, 10, 0, 0, -200, -400],
            "hpDeltaHold": [7, 7, 0, 0, -220, -460],
            "hpDeltaDrag": [7, 7, 0, 0, -220, -460],
            "hpDeltaRepeat": [10, 10, 0, 0, -200, -400],
            "hpDeltaBasicDuringFever": [10, 10, 10, 0, -200, -400],
            "hpDeltaChainDuringFever": [10, 10, 10, 0, -200, -400],
            "hpDeltaHoldDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaDragDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaRepeatDuringFever": [10, 10, 10, 0, -200, -400]
        },
        {
            "timeWindows": [12.5, 37.5, 51.25, 65, 83.75],
            "hpDeltaBasic": [7, 7, 0, 0, -200, -400],
            "hpDeltaChain": [7, 7, 0, 0, -200, -400],
            "hpDeltaHold": [7, 7, 0, 0, -220, -460],
            "hpDeltaDrag": [7, 7, 0, 0, -220, -460],
            "hpDeltaRepeat": [10, 10, 0, 0, -200, -400],
            "hpDeltaBasicDuringFever": [7, 7, 7, 0, -200, -400],
            "hpDeltaChainDuringFever": [7, 7, 7, 0, -200, -400],
            "hpDeltaHoldDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaDragDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaRepeatDuringFever": [10, 10, 10, 0, -200, -400]
        },
        {
            "timeWindows": [12.5, 37.5, 51.25, 65, 83.75],
            "hpDeltaBasic": [7, 7, 0, 0, -200, -400],
            "hpDeltaChain": [7, 7, 0, 0, -200, -400],
            "hpDeltaHold": [7, 7, 0, 0, -220, -460],
            "hpDeltaDrag": [7, 7, 0, 0, -220, -460],
            "hpDeltaRepeat": [7, 7, 0, 0, -200, -400],
            "hpDeltaBasicDuringFever": [7, 7, 7, 0, -200, -400],
            "hpDeltaChainDuringFever": [7, 7, 7, 0, -200, -400],
            "hpDeltaHoldDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaDragDuringFever": [7, 7, 7, 0, -220, -460],
            "hpDeltaRepeatDuringFever": [7, 7, 7, 0, -200, -400]
        }
    ],

    "timeWindowsInPulses": true,
    "longNoteGracePeriod": 0.1,
    "longNoteGracePeriodInPulses": false,

    "scanMarginTopBottom": [0.05, 0.05, 0.05],
    "scanMarginMiddle": [0.05, 0.05, 0.05],
    "scanMarginBeforeFirstBeat": 0.157,
    "scanMarginAfterLastBeat": 0.093,
    "hitboxWidth": 1.25,
    "hitboxHeight": 1.15,
    "chainHeadHitboxWidth": 100,
    "chainNodeHitboxWidth": 100,
    "dragHitboxWidth": 3,
    "dragHitboxHeight": 1.15,
    "ongoingDragHitboxWidth": 3,
    "ongoingDragHitboxHeight": 1.15,

    "maxHp": 10000,

    "comboBonus": true,

    "constantFeverCoefficient": true,
    "feverBonusOnMax": 1,
    "feverBonusOnCool": 1,
    "feverBonusOnGood": 0
}
```

5. In TECHMANIA, go to the options menu and set "ruleset" to "custom". If the game does not report an error, it means you have done the previous steps correctly.
6. Read about the parameters below and adjust them in `ruleset.json` to your liking. Make sure you write a comma after every line except the last parameter. If any parameter is missing, it will take the value from the standard ruleset.
7. Save `ruleset.json` and it will apply the next time you start a pattern, either from the main menu or from restarting/retrying. You do not need to restart TECHMANIA.

# Parameters

Below are descriptions of each parameter in the ruleset.

## Version

The ruleset format version. Must be "2".

## Windows and deltas

Short for time windows and HP deltas. These are the parameters that can be different whether you are playing individual patterns or a setlist; also different for each stage within a setlist. Time windows and HP deltas will be further explained in later sections.

`windowsAndDeltas` contains the windows and deltas when playing individual patterns; `windowsAndDeltasSetlist` is an array of 4 objects, each corresponding to one stage in a setlist. This means, the first set of windows and deltas under `windowsAndDeltasSetlist` will apply when you play the 1st pattern in a setlist, the second set applies on the 2nd pattern, and so on.

## Time windows

`timeWindows` is an array of 5 numbers (referred to as `timeWindows[0]` through `timeWindows[4]`), corresponding to the time window of Rainbow MAX, MAX, COOL, GOOD and MISS, respectively. All time windows are in seconds by default, but if `timeWindowsInPulses` is `true`, then they are in pulses. A pulse is 1/240 of a beat.

When you play a note, the game will compare the current time and the note's correct time, and reach a judgement as below.

![A diagram illustrating how the game makes judgements.](https://imgur.com/ghdWb0l.png)

If it's `timeWindows[4]` or more before a note's correct time, the game will ignore all input on the note. If it's `timeWindows[4]` past a note's correct time and the game still received no input on that note, it becomes a BREAK.

`longNoteGracePeriod` is a special window applied to long notes (hold, drag and repeat hold). It can also be in seconds or pulses, depending on `longNoteGracePeriodInPulses`. After a long note loses input, it will not cause a MISS until `longNoteGracePeriod` seconds or pulses has passed.

## HP and HP delta

**Watch out!** HP-related parameters must be integers.

HP deltas are defined as 10 arrays (`hpDeltaBasic` etc.); each array contains 6 numbers, corresponding to Rainbow MAX, MAX, COOL, GOOD, MISS and BREAK, respectively.

The player starts with `maxHp` points of HP. On each judgement, the game looks up the HP delta value from the array matching the note type, Fever status and judgement, then adds the delta to the current HP. A positive delta will increase HP; A negative delta will decrease HP.

## Hitbox sizes

Hitbox is the area where a note receives touches and clicks. Hitbox sizes are in multiples of a lane's height. During gameplay in practice mode, you can press F12 to show/hide the hitboxes.

![An example showing notes at different hitbox widths.](https://imgur.com/04e8IG6.png)

Notes that involve dragging (chain, drag) are harder to hit correctly than other notes, so they receive special rules. `chainHeadHitboxWidth` and `chainNodeHitboxWidth` define the hitbox width of chain heads and chain nodes, respectively. Drag notes' hitbox sizes are `dragHitboxWidth` by `dragHitboxHeight` at rest, but once the player starts dragging, the hitbox sizes change to `ongoingDragHitboxWidth` by `ongoingDragHitboxHeight`.

## Vertical scan margin

The game leaves a margin at the top and bottom of each scan to ensure notes don't touch the play area's border. The height of this margin is `scanMarginTopBottom` at the top of the top scan and the bottom of the bottom scan; `scanMarginMiddle` at the bottom of the top scan and the top of the bottom scan.

![A diagram showing the meaning of scanMarginTopBottom and scanMarginMiddle](https://imgur.com/llpBJ4X.png)

Furthermore, each parameter is an array of 3 elements, corresponding to 2-lane, 3-lane and 4-lane patterns, respectively.

All values are in multiples of a scan's height. By default, all margins are 5% of a scan's height, meaning the playable lanes take up a total of 90% of a scan's height.

## Horizontal scan margin

Likewise, the game leaves a margin before the first beat and after the last beat of each scan, to ensure notes don't touch the screen's border. The width of this margin is `scanMarginBeforeFirstBeat` before the first beat, and `scanMarginAfterLastBeat` after the last beat.

![A diagram showing the meaning of scanMarginBeforeFirstBeat and scanMarginAfterLastBeat](https://imgur.com/PiHpGiJ.png)

All values are in multiples of the screen's width. In the standard ruleset, the two margins are 15% and 10% of the screen's width respectively; in the legacy ruleset, they are 15.7% and 9.3%.

## Score and combo bonus

Each note adds to the score based on its judgement:

```
Judgement | Score
-------------------------------------------
R-MAX     | (R-MAX total) / number of notes
MAX       | R-MAX - 1
COOL      | R-MAX * 0.7
GOOD      | R-MAX * 0.4
MISS      | 0
BREAK     | 0
```

`comboBonus` can be set to `true` or `false` (no quotation marks), and defaults to `false`.

If `comboBonus` is `true`, `R-MAX total` is 290,000 points, and at the result screen the player will receive up to 10,000 points of Combo Bonus, calculated as follows:

* If the player achieved a Perfect Play (no COOL, GOOD, MISS or BREAK), the Combo Bonus is 10,000 points.
* Otherwise, Combo Bonus is `(7,800 * mb + 9,800) * (t - mb) / (mb + 1) / t` points, where `t` is the total number of notes, and `mb` is the sum of MISSes and BREAKs. This means a Full Combo gives 9,800 points, and Combo Bonus decreases with more MISSes and BREAKs.

If `comboBonus` is `false`, `R-MAX total` is 300,000 points, and there is no Combo Bonus.

In both cases, the maximum possible score is 300,000.

## Fever accumulation

Each Rainbow MAX or MAX increases Fever by `(Fever coefficient) / (number of notes)`. A full Fever bar corresponds to 1. The value of Fever coefficient depends on the `constantFeverCoefficient` parameter, which again can be `true` or `false`:

* If `constantFeverCoefficient` is `true`, Fever coefficient is 8. This means a Perfect Play will fill up the Fever bar 8 times, regardless of track length.
* If `constantFeverCoefficient` is `false`, Fever coefficient is `(track length in seconds) / 12.5`. This means a Perfect Play will fill up the Fever bar roughly every 12.5 seconds.

A MISS will decrease the current Fever by 25%; a BREAK by 50%.

Once activated, Fever lasts 10 seconds.

## Fever bonus

When Fever is active, MAX, COOL and GOOD judgements will receive `feverBonusOnMax`, `feverBonusOnCool`, `feverBonusOnGood` points in Fever bonus, respectively.

# Other parameters

The following parameters currently cannot be customized.

## Combo

Ongoing long notes (hold, drag, repeat hold) add 1 combo each 60 pulses (1/4 beat).

## Rank

At the result screen, the player will receive a rank based on their score as follows:
```
Score         | Rank
--------------------
Failed        | F
0-220000      | C
220001-260000 | B
260001-270000 | A
270001-280000 | A+
280001-285000 | A++
285001-290000 | S
290001-295000 | S+
295001-300000 | S++
```
For setlists, divide the score by 4 before using this table.