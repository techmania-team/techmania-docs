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

## 판정 수치

노트를 연주하면 게임 내에서 해당 노트의 정확한 플레이 시점과 연주 시각의 차이를 가지고 아래의 변수들을 토대로 판정을 결정합니다.
모든 판정수치의 단위는 s(초)입니다.

![A diagram illustrating how the game makes judgements.](https://imgur.com/8Skl6pB.png)

노트의 정확한 플레이 시점보다 `breakThreshold` 초만큼 경과했는데도 게임이 해당 노트에 대한 입력을 인지하지 못할 경우 BREAK가 발생합니다.

`longNoteGracePeriod`는 롱 노트(홀딩 노트, 드래그 노트, 연타 홀딩 노트를 가르킴)에만 특별히 적용되는 판정 수치입니다. 롱 노트의 마지막 시점으로부터 `longNoteGracePeriod` 초 이전까지는 해당 롱 노트에 대한 입력이 끊어져도 MISS 판정이 나지 않습니다.

## 히트박스 크기

히트박스는 각 노트가 터치 또는 마우스 클릭에 반응하는 영역을 의미합니다.
히트박스 크기 수치는 트랙 높이(기본적으로 스크롤 구간 높이의 22.5%로 설정되어 있음)의 배수값으로 나타냅니다.
모든 히트박스의 높이는 1입니다. (현재 히트박스의 너비를 트랙 높이의 몇 배로 할 것인지만 결정할 수 있습니다.)
게임 플레이 도중에 키보드의 F5 버튼을 눌러 히트박스를 표시하거나 숨길 수 있습니다.

![An example showing notes at different hitbox widths.](https://imgur.com/04e8IG6.png)

드래그 입력을 필요로 하는 연결형 노트와 드래그 노트의 경우 다른 노트들보다 정확히 입력하기 어려운 관계로 특수한 룰이 적용됩니다.
`chainHeadHitboxWidth`와 `chainNodeHitboxWidth`는 각각 연결형 노트 머리 부분과 연결형 노트 연결부의 히트박스 넓이를 정의합니다.
드래그 노트의 경우 첫 입력은 일반적인 노트의 히트박스와 동일하나, 드래그가 시작되어 유지되는 동안의 히트박스 너비와 높이는 `ongoingDragHitboxWidth` 및 `ongoingDragHitboxHeight`의 수치로 변경됩니다.

게임 내에서 트랙 간의 구분을 명확하게 하기 위해 각 트랙의 위와 아래에 여백이 있습니다. 이 위쪽 및 아래쪽 여백의 높이는 스크롤 구간 높이의 `scanMargin` 배입니다.
기본 수치 상 트랙들의 위쪽 및 아래쪽 여백은 스크롤 구간 높이의 5%로, 여백을 제외한 네 트랙의 영역을 합치면 총 스크롤 구간 높이의 90%를 차지한다는 뜻이 됩니다.

## 체력

**주의!** 모든 체력 관련 변수들은 정수값을 가져야 합니다.

플레이어는 게임 시작 시 `maxHp` 만큼의 체력을 갖게 되고, MISS 또는 BREAK가 발생할 때마다 `hpLoss`만큼 체력이 감소하는 반면, 다른 판정이 발생할 때마다 `hpRecovery`만큼 체력이 증가합니다.

피버 발동 중에는 MISS 또는 BREAK가 발생할 때마다 `hpLossDuringFever`만큼 체력이 감소하고, 다른 판정이 발생할 때마다 `hpRecoveryDuringFever`만큼 체력이 증가합니다.

## Score and combo bonus(점수 및 콤보 보너스)

각 노트는 판정에 따라 다음과 같은 점수를 부여합니다.

```
판정      | 점수
------------------------------------------------------------------------
R-MAX     | 만점(모든 노트에 R-MAX 판정을 받았을 때 얻는 점수) / 총 노트수
MAX       | R-MAX - 1
COOL      | R-MAX * 0.7
GOOD      | R-MAX * 0.4
MISS      | 0
BREAK     | 0
```

`comboBonus`는 `true` 또는 `false` 값(따옴표 미포함)을 가질 수 있으며 기본값은 `false`입니다.

`comboBonus` 값이 `true`인 경우 만점은 290,000점이 되며, 결과 화면에서 아래의 식으로 계산되는 콤보 보너스를 최대 10,000점까지 추가로 받게 됩니다.

* 플레이어가 Perfect Play를 달성한 경우 (COOL, GOOD, MISS, BREAK 판정이 하나도 발생하지 않음) 콤보 보너스는 10,000점입니다.
* Perfect Play 이외의 경우, 콤보 보너스는 `(7,800 * mb + 9,800) * (t - mb) / (mb + 1) / t` 점으로 계산됩니다. 여기서 `t`는 총 노트수, `mb`는 MISS 또는 BREAK 판정이 발생한 회수의 합입니다. 이는 All Combo 달성 시 콤보 보너스가 9,800점이 되며 MISS와 BREAK 판정이 많아질 수록 그 수치가 감소함을 나타냅니다.

`comboBonus` 값이 `false`인 경우 만점은 300,000점이 되며 콤보 보너스는 없습니다.

어느 경우에도 최종 점수는 300,000점 만점입니다.

## 피버 게이지

각 Rainbow MAX 또는 MAX는 `(피버 계수) / (총 노트수)`만큼 피버 게이지 수치를 증가시킵니다. 피버 게이지 수치가 1이 되면 피버 게이지가 가득 차게 됩니다. 피버 계수는 `constantFeverCoefficient` 변수에 의해 결정되며, 해당 변수는 `true` 또는 `false` 값을 가질 수 있습니다.

* `constantFeverCoefficient` 값이 `true`인 경우 피버 계수는 8이 됩니다. 이는 Perfect Play 달성 시 곡 길이와 상관없이 피버 게이지를 8회 가득 채울 만큼의 분량을 얻을 수 있다는 뜻입니다. 이 경우 노트 수가 적을수록 피버 게이지를 모으기 쉬워지고, 노트 수가 많으면 피버 게이지를 모으기 어려워집니다.
* `constantFeverCoefficient` 값이 `false`인 경우 피버 계수는 `(초 단위 곡 길이) / 12.5`가 됩니다. 이는 Perfect Play 달성 시 약 12.5초에 한번 꼴로 피버 게이지가 가득 차게 된다는 뜻입니다.

MISS 및 BREAK 판정이 발생하면 각각 현재 피버 게이지의 25% 및 50%에 해당하는 수치가 감소합니다.

피버의 발동 시간은 발동 시점으로부터 10초 동안입니다.

## 피버 보너스

피버 발동 중에 MAX, COOL, GOOD 판정은 기존의 점수에 더해 각각 `feverBonusOnMax`, `feverBonusOnCool`, `feverBonusOnGood`만큼의 피버 보너스 점수를 추가로 부여하게 됩니다.

# 기타 변수들

현재 아래의 변수들은 임의로 조정할 수 없습니다.

## 콤보

롱 노트(홀딩 노트/드래그 노트/연타 홀딩 노트)의 경우 60틱(= 1/4박)에 1 콤보씩 콤보를 누적시킵니다.

## 랭크 조건

플레이어가 얻은 최종 점수에 따라 결과화면에 보여지는 랭크가 아래와 같이 결정됩니다.

```
점수            | 랭크
----------------------
Failed(게임오버) | F
0-220000        | C
220001-260000   | B
260001-270000   | A
270001-280000   | A+
280001-285000   | A++
285001-290000   | S
290001-295000   | S+
295001-300000   | S++
```
