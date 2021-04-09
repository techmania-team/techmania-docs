기준 버전 : 0.4

룰셋은 TECHMANIA의 난이도를 결정하는 변수들의 모음입니다.
본 게임은 내부적으로 고정된 룰셋을 사용하지만 플레이어가 지정하는 별도의 룰셋으로 이를 덮어  수 있습니다.

# 룰셋을 새로 정의하는 방법

1. 우선 TECHMANIA를 실행하여 옵션 메뉴를 한번이라도 열어보아야 합니다. (옵션 파일을 자동으로 생성하기 위함입니다.)
2. 파일 탐색기에서 문서 폴더를 열어 `TECHMANIA` 폴더에 들어갑니다.
3. 해당 폴더는 대체로 `C:\Users\<사용자 PC명>\OneDrive\Documents\TECHMANIA` 또는 `C:\Users\<사용자 PC명>\Documents\TECHMANIA`에 위치합니다.
4. 그 폴더에서 `options.json` 파일을 찾을 수 있습니다.
5. `options.json` 파일과 동일한 경로에 `ruleset.json`이라는 이름의 새로운 파일을 생성합니다.
6. 텍스트 편집기로 `ruleset.json`을 열고 아래의 내용을 중괄호를 포함하여 복사 후 붙여넣기 한 다음 파일을 저장합니다.

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

위에 나타난 수치들은 0.4 버전에서 기본으로 사용하는 룰셋의 수치입니다.

7. 파일이 제대로 저장되었을 경우, 이후부터 TECHMANIA 실행 시 게임 우측 하단에 "Custom ruleset active"라는 문구가 표시됩니다.
8. 아래에 기술된 각 변수들의 상세설명을 숙지하고 원하는 수치를 `ruleset.json`에 기입합니다. 마지막 변수를 제외한 모든 줄에 콤마(,)를 제대로 입력했는지 확인합니다.
9. `ruleset.json` 파일을 저장하면 이후부터 패턴을 플레이(메인 메뉴에서 패턴을 선택하여 플레이 또는 플레이하던 중인 패턴 재시작)하는 순간부터 기입된 수치가 적용됩니다. TECHMANIA를 재시작할 필요는 없습니다.
10. 게임이 사용하던 기존 룰셋으로 다시 되돌아가고 싶은 경우, `ruleset.json`을 `ruleset.json.renamed` 등의 다른 이름으로 변경해줍니다. 나중에 다시 자신이 정의한 룰셋을 적용시키려면 해당 파일의 이름을 원래대로 `ruleset.json`이 되도록 변경합니다. `ruleset.json`에서 특정 변수가 기입된 줄들을 제거하면 해당 변수들은 게임 내 기본 룰셋의 수치를 사용하게 됩니다.

# 변수 설명

아래에 룰셋에 포함된 각 변수들에 대한 설명을 기술합니다.

## Version(버전)

룰셋 형식 버전입니다. 현재 "1"로 값을 고정해두어야 합니다.

## Timing windows(판정 수치)

노트를 연주하면 게임 내에서 노트의 정확한 플레이 시점과 연주 시각을 비교하여 그 차이로 아래의 and reach a judgement as below. All time windows are in seconds.

![A diagram illustrating how the game makes judgements.](https://imgur.com/8Skl6pB.png)

If it's `breakThreshold` seconds past a note's correct time and the game still received no input on that note, it becomes a BREAK.

`longNoteGracePeriod` is a special window applied to long notes (hold, drag and repeat hold). For the last `longNoteGracePeriod` seconds during a long note, missing input will not cause a MISS.

## Hitbox sizes

Hitbox is the area where a note receives touches and clicks. Hitbox sizes are in multiples of a lane's height (which is by default 22.5% of a scan's height); all hitboxes have a height of 1. During gameplay, you can press F5 to show/hide the hitboxes.

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

* If `constantFeverCoefficient` is `true`, Fever coefficient is 8. This means a Perfect Play will fill up the Fever bar 8 times, regardless of track length. Please be aware that this may make Fever too easy to accumulate on shorter tracks, and too difficult on longer tracks.
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
