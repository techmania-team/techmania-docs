기준 버전: 1.2

PT 파일의 트랙과 노트 형식, 속성에 대한 설명입니다. 

## 트랙
* 0-3: 1P 플레이 영역으로, TECHNIKA의 모든 모드에서 사용하는 트랙입니다. TECHMANIA의 0-3번 트랙으로 매핑됩니다.
* 4-7: 1P 확장 영역으로, 0-3번 트랙의 노트가 화면에 출력되는 방식을 지정하는 영역입니다.
* 8-15: 2P 영역으로, TECHNIKA 2의 Duo Mixing에서 사용하는 트랙입니다. TECHMANIA에서는 이 트랙을 무시합니다.
* 16-: 배경음 트랙입니다. TECHMANIA의 4번 트랙부터 매핑됩니다. 트랙 번호가 64 이상이 될 경우 패턴을 변환할 수 없습니다.

## 노트 형식
* 1: 일반 노트.
* 2: 볼륨 노트. 각 트랙의 볼륨을 지정합니다.
* 3: 템포 노트. 변속을 지정합니다.
* 4: 박자 노트. 변박을 지정합니다. TECHMANIA에서는 지원하지 않습니다.

## 노트 속성(플레이 트랙)
* 0, 5, 6, 10, 11, 12: TECHNIKA에서 사용하는 노트 속성으로, TECHMANIA에서도 각 노트의 속성을 유지합니다.
* 100: BGA의 시작점으로, TECHMANIA의 각 패턴 헤더의 BGA 오프셋으로 변환됩니다.

## 노트 속성(확장 트랙)
* 확장 트랙의 노트는 자신이 속한 트랙보다 4개 앞선 트랙의 노트 출력 방식을 바꿉니다.
* 스크롤 영역의 시작 위치에 있는 노트의 4트랙 뒤에 일반 노트가 있으면, 해당 노트는 이전 스크롤 영역의 맨 마지막 노트가 됩니다.
* 드래그 노트의 4트랙 뒤에 일반 노트가 있으면, 이 노트의 곡선 모양이 바뀝니다. 속성 값이 60이면 평행하고, 60보다 작으면 위로 올라가며, 크면 아래로 내려갑니다.
