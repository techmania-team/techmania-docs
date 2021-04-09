기준 버전 : 0.4

이 페이지에서는 TECHMANIA에서 사용할 수 있는 스킨을 제작하는 방법에 대해 설명합니다.

# 스킨 타입 및 위치 경로

현재 TECHMANIA에서 커스터마이징을 지원하는 스킨은 3종류입니다.
* Note(노트 스킨) : 노트의 모습을 결정합니다. `<TECHMANIA 설치 폴더>/Skins/Note/<노트 스킨 이름>`에 위치합니다.
* VFX(쿨밤 스킨) : 노트가 연주될 때의 폭발 효과(쿨밤) 및 피버 발동 시 노트에 추가되는 효과를 결정합니다. `<TECHMANIA 설치 폴더>/Skins/VFX/<쿨밤 스킨 이름>`에 위치합니다.
* Combo(콤보 폰트 스킨) : 연주한 노트의 위쪽에 나타나 현재 콤보수와 노트의 판정을 표시하는 콤보 텍스트의 모습을 결정합니다. `<TECHMANIA 설치 폴더>/Skins/Combo/<콤보 폰트 스킨 이름>`에 위치합니다.

각 스킨은 아래에 후술할 `skin.json`과 몇 가지 스프라이트 시트를 포함하는 폴더입니다.

# 스프라이트 시트

모든 스킨은 스프라이트 시트를 기본으로 하므로 이를 먼저 이해하는 것이 중요합니다.
스프라이트 시트의 정의는 관련된 분야에 따라 다를 수 있지만, TECHMANIA의 스킨에 한해서는 애니메이션의 한 프레임에 해당하는 각각의 이미지를 가로로 나열하여 하나의 이미지로 묶은 것으로 정의합니다.
각 이미지(스프라이트)는 크기가 모두 동일해야하며 아무 여백 없이 서로 붙어있는 채로 연결되어야 합니다.

![An image showing the difference between a sprite and a sprite sheet](https://imgur.com/VoOllqf.png)

(위 예시에 보이는 스프라이트 시트의 출처는 다음과 같습니다. [https://www.gamedesigning.org/animation/sprites/](https://www.gamedesigning.org/animation/sprites/))

스프라이트 시트의 모든 타일이 스프라이트를 가지고 있어야 하는 것은 아닙니다.
스프라이트 시트의 처음이나 끝에 몇 개의 빈 이미지를 집어넣는 경우도 있습니다.

![A sprite sheet with empty tiles at the beginning and end](https://imgur.com/QY62oea.png)

각 스프라이트 시트에 대해 이를 TECHMANIA에서 불러들일 수 있도록 하는 상세설명을 담은 JSON 객체파일을 다음과 같은 형식으로 작성해야 합니다.
```
{
  "filename": <파일명>,
  "rows": <타일 행 수>,
  "columns": <타일 열 수>,
  "firstIndex": <스프라이트를 포함하는 첫 타일의 인덱스>,
  "lastIndex": <스프라이트를 포함하는 마지막 타일의 인덱스>,
  "bilinearFilter": <true 또는 false>
}
```

타일 인덱스는 0부터 시작하고 행 기준으로 정렬되어 있습니다.
예를 들어, 위에 예시로 첨부한 이미지 중 두번째 이미지에 해당하는 스프라이트 시트의 JSON 객체파일은 다음과 같습니다.
```
{
  "filename": "sprite sheet.png",
  "rows": 3,
  "columns": 4,
  "firstIndex": 1,
  "lastIndex": 10
}
```
이 경우 첫째 행은 0번~3번 타일, 둘째 행은 4번~7번 타일, 셋째 행은 8번~11번 타일을 포함합니다.
스프라이트가 들어가 있는 타일은 1번~10번 타일입니다.

`bilinearFilter`는 스프라이트를 다른 크기로 변경해서 적용시킬 때 Bilinear filter(2중선형보간필터)를 적용할 지의 유무를 나타냅니다.
해당 필터를 사용하면 크기가 변경된 스프라이트가 더 부드러워 보이지만, 스프라이트 경계의 바깥쪽에 있는 픽셀들을 읽어들이므로 배경에 투명처리를 제대로 하지 않은 스프라이트의 경우 모서리가 왜곡되어 보일 수 있습니다.
이 경우 필터를 끄는 편이 더 나을 수도 있습니다.

![An image showing the same sprite scaled with bilinear filter on and off](https://imgur.com/8mUvHxi.png)
![An image showing the same sprite sheet scaled with bilinear filter on and off, and producing an artifact when on](https://imgur.com/n6nWt55.png)

파일명을 제외한 나머지 항목들은 필수가 아니며, 생략할 경우 다음과 같은 값들이 자동으로 채워집니다.
* `rows`: 1
* `columns`: 1
* `firstIndex`: 0
* `lastIndex`: 0
* `bilinearFilter`: true

즉 파일명 이외에 아무것도 기재하지 않는 경우 TECHMANIA는 이를 하나의 큰 스프라이트 이미지로 인식하고 로드하게 됩니다.

각 스킨 타입별로 JSON 객체파일에 추가적으로 기재해야할 내용에 대해서는 아래에 후술합니다.

# 노트 스킨
노트스킨의 `skin.json` 파일의 경우 아래의 형식을 따릅니다.
```
{
  "version": "1",
  "basic": <노트>,
  "chainHead": <연결형 노트 머리 부분>,
  "chainNode": <연결형 노트 연결부>,
  "chainPath": <연결형 노트 진행 경로 배경>,
  "dragHead": <드래그 노트 머리 부분>,
  "dragCurve": <드래그 노트 진행 경로>,
  "holdHead": <홀딩 노트 머리 부분>,
  "holdTrail": <홀딩 노트 경로>,
  "holdTrailEnd": <홀딩 노트 경로 끝 부분>,
  "holdOngoingTrail": <홀딩 노트를 누르고 있을 때의 경로>,
  "repeatHead": <연타 노트 머리 부분>,
  "repeat": <연타 노트>,
  "repeatHoldTrail": <연타 홀딩 노트 경로>,
  "repeatHoldTrailEnd": <연타 홀딩 노트 경로 끝 부분>,
  "repeatPath": <연타 노트 연결선>
}
```
각 값들은 스프라이트 시트를 가르킵니다. 각 줄이 의미하는 바를 정확히 파악하려면 [용어집](Terminology.md) 페이지를 참고하세요.

노트 스킨에 들어가는 대부분의 스프라이트 시트들의 경우 `scale`이라는 별도의 항목을 추가적으로 사용합니다.
* `basic`, `chainHead`, `chainNode`, `dragHead`, `holdHead`, `repeatHead`, `repeat` 스프라이트의 경우 `scale * 트랙 높이`만큼의 너비와 높이를 갖는 정사각형에 맞춰 크기가 조정됩니다. 이는 어디까지나 시각적인 조정이며 실제 히트박스 크기에는 아무런 영향을 주지 않습니다.
* `chainPath`, `dragCurve`, `holdTrail`, `holdOngoingTrail`, `repeatHoldTrail`, `repeatPath` 스프라이트의 경우 게임 내에서 해당 스프라이트가 선을 이루도록 노트 특성에 따라 가로로/세로로/노트에서 노트로/곡선모양으로 확장됩니다. according to the pattern, so the sprite's size in that dimension is unaffected by `scale`. The size in the perpendicular dimension will be scaled to `scale * lane height`.
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

# VFX skin
The `skin.json` file in a VFX skin follows the following format:
```
{
  "version": "1",
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

![An image demonstrating normal and additive shaders](https://imgur.com/oR6gHqA.png)

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
