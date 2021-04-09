기준 버전 : 0.4

이 페이지에서는 TECHMANIA에서 사용할 수 있는 스킨을 제작하는 방법에 대해 설명합니다.

# 스킨 타입 및 위치 경로

현재 TECHMANIA에서 커스터마이징을 지원하는 스킨은 3종류입니다.
* Note(노트 스킨) : 노트의 모습을 결정합니다. `<TECHMANIA 설치 폴더>/Skins/Note/<노트 스킨 이름>`에 위치합니다.
* VFX(쿨밤 스킨) : 노트가 연주될 때의 폭발 효과(쿨밤) 및 피버 발동 시 노트에 추가되는 효과를 결정합니다. `<TECHMANIA 설치 폴더>/Skins/VFX/<쿨밤 스킨 이름>`에 위치합니다.
* Combo(콤보 스킨) : 연주한 노트의 위쪽에 나타나 현재 콤보수와 노트의 판정을 표시하는 콤보 텍스트의 모습을 결정합니다. `<TECHMANIA 설치 폴더>/Skins/Combo/<콤보 스킨 이름>`에 위치합니다.

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
노트 스킨의 `skin.json` 파일은 아래의 형식을 따릅니다.
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
* `chainPath`, `dragCurve`, `holdTrail`, `holdOngoingTrail`, `repeatHoldTrail`, `repeatPath` 스프라이트의 경우 게임 내에서 해당 스프라이트가 선을 이루도록 노트 특성에 따라 가로로/세로로/노트에서 노트로/곡선모양으로 확장됩니다. 스프라이트의 크기 자체는 `scale`의 영향을 받지 않지만, 선을 이루는 방향에 수직인 너비는 `scale * 트랙 높이` 값을 갖게 됩니다.
* `holdTrailEnd` 및 `repeatHoldTrailEnd`의 경우 `scale` 항목을 사용하지 않습니다.
* `scale` 항목을 생략하는 경우 1을 기본값으로 사용합니다.

![An image demonstrating the scale of repeat head and repeat hold trail](https://imgur.com/m1EO3jo.png)

`dragCurve` 스프라이트의 경우 특이한 방식으로 처리됩니다. 스프라이트 이미지의 왼쪽 절반은 드래그 노트 진행 경로의 몸체를 이루도록 확장되고, 스프라이트 이미지의 오른쪽 절반은 앞에서 확장된 왼쪽 절반의 스프라이트 이미지에 붙어서 드래그 노트가 끝나는 부분을 나타내게 됩니다.

![An image demonstrating a drag curve being stretched in the left half](https://imgur.com/1wZbgA6.png)

`holdTrailEnd` 및 `repeatHoldTrailEnd` 스프라이트는 각자 홀딩 노트와 연타 홀딩 노트의 경로를 나타내는데 쓰이며, 주로 그림자 및 번쩍임 효과를 내기 위해 쓰입니다.
해당 스프라이트의 높이는 대응하는 노트에 맞춰서 자동으로 조정되며, 스프라이트 이미지 자체의 높이:너비 비율은 유지됩니다.

![An image demonstrating a repeat hold trail and its end](https://imgur.com/lNSPcJk.png)

`holdOngoingTrail` 스프라이트는 판정 라인이 지나간 이후의 홀딩 노트의 모습을 나타냅니다.

![An image demonstrating a hold ongoing trail](https://imgur.com/6Vib3rT.png)

모든 스프라이트 시트의 애니메이션은 1박 당 한번의 사이클을 이루며 보여지고, 각 스프라이트 시트의 첫 스프라이트 이미지가 박자가 시작됨과 동시에 보여지는 이미지가 됩니다.

# 쿨밤 스킨

쿨밤 스킨의 `skin.json` 파일은 아래의 형식을 따릅니다.
```
{
  "version": "1",
  "feverOverlay": <피버 발동 시 노트 추가 효과>,
  "basicMax": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "basicCool": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "basicGood": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "dragOngoing": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "dragComplete": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "holdOngoingHead": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "holdOngoingTrail": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "holdComplete": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "repeatHead": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "repeatNote": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "repeatHoldOngoingHead": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "repeatHoldOngoingTrail": [<레이어 0>, <레이어 1>, ..., <레이어 n>],
  "repeatHoldComplete": [<레이어 0>, <레이어 1>, ..., <레이어 n>]
}
```

쿨밤 스킨에서는 피버 오버레이(feverOverlay)를 제외한 모든 효과들이 다수의 레이어들로 이루어져 있으며 각 레이어는 하나의 스프라이트 시트에 대응합니다. (여러 판정들에 각각 대응하는 스프라이트들을 모두 포함합니다.)
레이어는 각기 정의된 순서대로 그려지며 맨 마지막에 오는 레이어가 제일 위쪽에 표시됩니다.
효과를 나타내는 레이어가 없거나 1개 뿐이어도 위의 구문에서 대괄호(`[]`)를 생략할 수 없습니다.

쿨밤 스킨에 들어가는 각 스프라이트 시트의 경우 추가로 아래의 3가지 항목을 갖습니다.
* `scale` 항목은 쿨밤의 크기를 결정합니다. 모든 스프라이트 이미지는 `scale * 트랙 높이` 만큼의 높이를 가지게 되고, 스프라이트 이미지 자체의 높이:너비 비율은 유지됩니다. 해당 항목을 생략할 경우 1을 기본값으로 사용합니다.
* `speed` 항목은 애니메이션의 속도를 60 fps의 배수로 결정합니다. 예를 들어 `speed`가 0.5인 경우, 쿨밤 애니메이션은 30 fps로 재생됩니다. 해당 항목을 생략할 경우 1을 기본값으로 사용합니다.
* `additiveShader` 항목은 TECHMANIA가 스프라이트를 그릴 때 Additive Shader(밝기 쉐이더)를 적용할 것일지에 대한 유무를 결정합니다. 쉐이더를 적용할 경우 새로 그려지는 레이어가 기존에 그 아래에 위치한 레이어들을 완전히 가려버리고 대체하는 것이 아니라 서로 겹쳐져서 색상을 더해주는 역할을 하게 됩니다. 해당 항목을 생략할 경우 `false`를 기본값으로 사용합니다.

![An image demonstrating normal and additive shaders](https://imgur.com/oR6gHqA.png)

각 효과별 상세설명과 애니메이션의 루프 여부는 다음과 같습니다.
* `feverOverlay`: 피버가 발동 중일 때 특정 노트들에 가해지는 추가 효과입니다. (애니메이션이 루프합니다.)
* `basicMax` / `basicCool` / `basicGood`: 각각 노트 또는 연결형 노트를 연주하여 MAX/COOL/GOOD 판정이 발생할 때 나타나는 효과입니다. (애니메이션이 한 번만 재생됩니다.)
* `dragOngoing`: 드래그 노트가 연주되는 중에 드래그 노트 머리 부분에 나타나는 효과입니다. (애니메이션이 루프합니다.)
* `dragComplete`: 드래그 노트 연주가 끝나는 시점에 드래그 노트 진행 경로 끝 부분에 나타나는 효과입니다. (애니메이션이 한 번만 재생됩니다.)
* `holdOngoingHead` / `holdOngoingTrail`: 각각 홀딩 노트가 연주되는 중에 홀딩 노트 머리 부분 및 타임라인이 홀딩 노트를 지나가는 부분에 나타나는 효과입니다. (애니메이션이 루프합니다.)
* `holdComplete`: 홀딩 노트 연주가 끝나는 시점에 홀딩 노트 경로 끝 부분에 나타나는 효과입니다. (애니메이션이 한 번만 재생됩니다.)
* `repeatHead` / `repeatNote`: 각각 연타 노트 머리 부분 및 연타 노트를 연주할 때 나타나는 효과입니다. (애니메이션이 한 번만 재생됩니다.)
* `repeatHoldOngoingHead` / `repeatHoldOngoingTrail`: 각각 연타 홀딩 노트가 연주되는 중에 연타 노트 머리 부분 및 타임라인이 연타 홀딩 노트를 지나가는 부분에 나타나는 효과입니다. (애니메이션이 루프합니다.)
* `repeatHoldComplete`: 연타 홀딩 노트 연주가 끝나는 시점에 연타 홀딩 노트 경로 끝 부분에 나타나는 효과입니다. (애니메이션이 한 번만 재생됩니다.)

# 콤보 스킨
콤보 스킨의 `skin.json` 파일은 아래의 형식을 따릅니다.
```
{
  "version": "1",
  "distanceToNote": <노트와의 거리>,
  "height": <높이>,
  "spaceBetweenJudgementAndCombo": <판정 텍스트와 콤보 폰트 사이의 거리>,
  "feverMaxJudgement": <Fever MAX 판정시 콤보 폰트>,
  "rainbowMaxJudgement": <Rainbow MAX 판정시 콤보 폰트>,
  "maxJudgement": <MAX 판정시 콤보 폰트>,
  "coolJudgement": <COOL 판정시 콤보 폰트>,
  "goodJudgement": <GOOD 판정시 콤보 폰트>,
  "missJudgement": <MISS 판정시 콤보 폰트>,
  "breakJudgement": <BREAK 판정시 콤보 폰트>,
  "feverMaxDigits": [<숫자 폰트 0>, <숫자 폰트 1>, ..., <숫자 폰트 9>],
  "rainbowMaxDigits": [<숫자 폰트 0>, <숫자 폰트 1>, ..., <숫자 폰트 9>],
  "maxDigits": [<숫자 폰트 0>, <숫자 폰트 1>, ..., <숫자 폰트 9>],
  "coolDigits": [<숫자 폰트 0>, <숫자 폰트 1>, ..., <숫자 폰트 9>],
  "goodDigits": [<숫자 폰트 0>, <숫자 폰트 1>, ..., <숫자 폰트 9>]
}
```

각 판정시 콤보 폰트와 숫자 폰트는 스프라이트 시트로 이루어져 있습니다. 콤보 스킨이 포함하는 각 스프라이트 시트의 경우 `speed` 항목을 추가적으로 가지며, 이는 쿨밤 스킨에서 사용하는 것과 같은 방식으로 애니메이션에 영향을 줍니다.

콤보 스킨은 판정 텍스트, 가로 방향 간격, 현재 콤보 수를 최대 4자리까지 표시하는 콤보 폰트의 3가지 부분으로 구성됩니다. MISS 또는 BREAK 판정의 경우 숫자 폰트는 나타나지 않으며, 다른 판정의 경우 해당되는 판정의 숫자 폰트가 나타납니다.

다음 이미지는 `distanceToNote`, `height`, `spaceBetweenJudgementAndCombo`가 가르키는 바를 보여줍니다.

![An image showing the meaning of distanceToNote, height and spaceBetweenJudgementAndCombo](https://imgur.com/J3VP1qF.png)

3가지 항목 모두 픽셀 수(정수값)입니다. 해상도와 관계없이 게임의 창 높이는 1080 픽셀로 고정된 것으로 간주하고, As a reference, the game window's height is always 1080 pixels regardless of resolution. All sprites in the combo text will be scaled so that their heights are equal to `height`, while keeping their aspect ratios unchanged.

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
