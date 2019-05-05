# 2048 is YOU
2048 game implemented by Baba is You

## 조작 방법
IS가 위쪽 ICE 바로 아래 줄에 있을 때, 방향키를 눌러 조작합니다. 방향키를 누르지 않으면 기본값인 DOWN으로 조정됩니다. SPACE, ENTER 등 아무 키나 누르면 IS가 cycle을 돌면서 이동이 처리됩니다.

## 설명
Baba is You의 editor로 무엇이든 만들어내는 걸 보면서, 2048을 만들어보고 싶다는 생각이 갑자기 들어서 만들어봤습니다.
전체 이동 cycle은 크게 5개의 phase로 구성됩니다. 위쪽 ICE가 있는 문장이 1번, LONELY BRICK TELE가 18번입니다.

- phase 1(문장 1-2): 방향 지정 phase. 1번 문장에서 캐릭터가 원래대로 바뀌고, 2번에서 유저가 방향을 정합니다.

- phase 2(문장 3-4): 처리 phase 1. 랜덤으로 2(BABA)를 만들었던 흔적들을 지웁니다.

- phase 3(문장 5-10): 2048 이동 phase. 2048의 룰대로 이동합니다. 이동하거나 합쳐지면 HAND가 생성됩니다.

- phase 4(문장 11-15): 랜덤 생성 setting phase. HEDGE에서 BRICK이 생성됩니다.

- phase 5( 16-18): 랜덤 생성 phase. HAND가 만들어졌다면, FUNGUS가 있는 위치에 BABA가 생성됩니다. 18번 문장에서 이 BABA가 비어있는 칸 중 아무 위치에 생성됩니다.

## Version 1.0
![2048 is you Version 1.0](/ver-1.0.png)

## Version 1.1
![2048 is you Version 1.1](/ver-1.1.png)
- 버그 수정: FUNGUS에서 만들어진 BABA가 BELT를 탈출하여 맵을 자유롭게 돌아다닐 수 있었던 부분을 수정
