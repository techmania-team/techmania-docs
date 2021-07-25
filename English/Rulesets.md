Applies to version: 0.9

A ruleset is a collection of parameters that determine the difficulty of TECHMANIA. The game contains a built-in, default ruleset, but you can override it with a custom ruleset.

# How to define custom ruleset

1. Make sure you have launched TECHMANIA and visited the options menu at least once.
2. In File Explorer, look for a folder called `TECHMANIA` in your documents folder. It's usually located at `C:\Users\<username>\OneDrive\Documents\TECHMANIA` or `C:\Users\<username>\Documents\TECHMANIA`. You should see a file called `options.json` here.
3. Next to `options.json`, create a new file called `ruleset.json`.
4. Open `ruleset.json` with any text editor, paste in the following content (including the braces), and save:

```
{
    "version": "1",

    "rainbowMaxWindow": 0.04,
    "maxWindow": 0.07,
    "coolWindow": 0.1,
    "goodWindow": 0.15,
    "breakThreshold": 0.3,
    "longNoteGracePeriod": 0.15,

    "scanMargin": 0.05,
    "hitboxWidth": 1.5,
    "chainHeadHitboxWidth": 1.5,
    "chainNodeHitboxWidth": 3,
    "ongoingDragHitboxWidth": 2,
    "ongoingDragHitboxHeight": 2,

    "maxHp": 1000,
    "hpLoss": 50,
    "hpRecovery": 3,
    "hpLossDuringFever": 50,
    "hpRecoveryDuringFever": 5,

    "comboBonus": false,

    "constantFeverCoefficient": false,
    "feverBonusOnMax": 1,
    "feverBonusOnCool": 1,
    "feverBonusOnGood": 0
}
```

Note that these values are from the default ruleset as of 0.4.

5. If done correctly, the next time you launch TECHMANIA, you should see "Custom ruleset active" on the bottom-right corner.
6. Read about the parameters below and adjust them in `ruleset.json` to your liking. Make sure you write a comma after every line except the last parameter.
7. Save `ruleset.json` and it will apply the next time you start a pattern, either from the main menu or from restarting/retrying. You do not need to restart TECHMANIA.
8. If you need to revert to the default ruleset, you can rename `ruleset.json` to something else, such as `ruleset.json.renamed`. When you need the custom ruleset again, rename the file back to `ruleset.json`. You can also remove a parameter's line from `ruleset.json` to use the default value on that parameter.

# Parameters

Below are descriptions of each parameter in the ruleset.

## Version

The ruleset format version. Must be "1".

## Timing windows

When you play a note, the game will compare the current time and the note's correct time, and reach a judgement as below. All time windows are in seconds.

![A diagram illustrating how the game makes judgements.](https://imgur.com/8Skl6pB.png)

If it's `breakThreshold` seconds past a note's correct time and the game still received no input on that note, it becomes a BREAK.

`longNoteGracePeriod` is a special window applied to long notes (hold, drag and repeat hold). For the last `longNoteGracePeriod` seconds during a long note, missing input will not cause a MISS.

## Hitbox sizes

Hitbox is the area where a note receives touches and clicks. Hitbox sizes are in multiples of a lane's height (which is by default 22.5% of a scan's height); all hitboxes have a height of 1. During gameplay in practice mode, you can press F12 to show/hide the hitboxes.

![An example showing notes at different hitbox widths.](https://imgur.com/04e8IG6.png)

Notes that involve dragging (chain, drag) are harder to hit correctly than other notes, so they receive special rules. `chainHeadHitboxWidth` and `chainNodeHitboxWidth` define the hitbox width of chain heads and chain nodes, respectively. Drag notes have normal hitboxes at rest, but once the player starts dragging, the hitbox size changes to `ongoingDragHitboxWidth` and `ongoingDragHitboxHeight`.

The game leaves a margin at the top and bottom of each scan in order to distinguish notes in the top and bottom scans more clearly. The height of this margin is `scanMargin` of a scan's height on each side. By default, the margin is 5% on each side, meaning the 4 lanes take up a total of 90% of a scan's height.

## HP

**Watch out!** HP-related parameters must be integers.

The player starts with `maxHp` points of HP, each MISS or BREAK decreases HP by `hpLoss` points, and other judgements increase HP by `hpRecovery` points.

When Fever is active, each MISS or BREAK decreases HP by `hpLossDuringFever` points, and other judgements increase HP by `hpRecoveryDuringFever` points.

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
