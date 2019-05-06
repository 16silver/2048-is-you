# 2048 is YOU
2048 game implemented by Baba is You

## 조작 방법
`IS`가 위쪽 `ICE` 바로 아래 줄에 있을 때, 방향키를 눌러 조작합니다. 방향키를 누르지 않으면 기본값인 `DOWN`으로 설정됩니다. `SPACE`, `ENTER` 등 아무 키나 누르면 `IS`가 cycle을 돌면서 이동이 처리됩니다.

## 설명
Baba is You의 editor로 무엇이든 만들어내는 걸 보면서, 2048을 만들어보고 싶다는 생각이 갑자기 들어서 만들어봤습니다.

2048의 2에 해당하는 것이 `BABA`, 4에 해당하는 것이 `KEKE`, 이런 식입니다. 정확히는 다음과 같습니다.
뒤쪽에 있는 건 2048 이동 과정에서 필요한 *alternative form*입니다.(뒤에서 설명)

- 2: `BABA / ROSE`
- 4: `KEKE / LAVA`
- 8: `FLAG / KEY`
- 16: `TREE / GRASS`
- 32: `CLOUD / WATER`
- 64: `ME / FLOWER`
- 128: `GHOST / LOVE`
- 256: `ROCK / BOX`
- 512: `ORB / SKULL`
- 1024: `MOON / DUST`
- 2048: `STAR`

### Version 1.0 ~ 1.1

전체 이동 cycle은 크게 5개의 phase로 구성됩니다. 위쪽 `ICE`가 있는 문장이 1번, `LONELY BRICK (IS) TELE`가 18번입니다.

- phase 1(문장 1-2): 방향 지정 phase. 1번 문장에서 캐릭터가 원래대로 바뀌고, 2번에서 유저가 방향을 정합니다.

- phase 2(문장 3-4): 처리 phase 1. 랜덤으로 2(`BABA`)를 만들었던 흔적들을 지웁니다.
  - 정확히는 `BRICK`을 없애고, 그 다음 랜덤 생성 위치의 `BABA`를 없앱니다.

- phase 3(문장 5-10): 2048 이동 phase. 2048의 룰대로 이동합니다. 이동하거나 합쳐지면 `HAND`가 생성됩니다.
  - 이 부분이 핵심인데, **`FACING`** 을 이용하여 2048의 이동을 구현하였습니다.
  - 오브젝트가 벽을 바라보고 있거나, 벽을 바라보는 것을 바라보고 있는 등, 더 움직이면 안 되는 상황에서는 *alt form*으로 바뀌게 됩니다.
  - 오브젝트 X와 alt(X)가 만나게 되면 **다음 단계의 오브젝트의 alt form**을 만들고, 원래의 두 물체는 사라집니다. 이 때 `HAND`가 생깁니다.
  - 오브젝트가 `EMPTY`를 바라보고 있으면 `MOVE`를 합니다. 이 때도 `HAND`가 생깁니다.
  - `HAND`는 `BELT`를 타고 아래의 `FUNGUS` 위치로 이동합니다. **이 `HAND`는, 2048의 룰에 의한 이동이 일어났는지를 판단하는 기준이 됩니다.**

- phase 4(문장 11-15): 랜덤 생성 setting phase. `HEDGE`에서 `BRICK`이 생성됩니다.
  - `HEDGE FACING HEDGE MAKE BRICK`이므로, 사진에 있는 맵에서 `HEDGE`의 방향을 정하는 것은 매우 중요합니다.
  - 4×4가 아닌 **6×6(`HEDGE`가 있는 곳 포함)이 모두 `BRICK`이 되어야 합니다.** `BRICK`에는 `STOP` 속성이 붙어있어 방향을 정할 때 오브젝트들이 움직이지 않도록 합니다.
  - 따라서 위의 6개의 `HEDGE`만 `HEDGE`를 바라보도록 합니다.

- phase 5(문장 16-18): 랜덤 생성 phase. `HAND`가 만들어졌다면, `FUNGUS`가 있는 위치에 `BABA`가 생성됩니다. 18번 문장에서 이 `BABA`가 비어있는 칸 중 아무 위치로 옮겨갑니다.
  - `HAND`가 생성되지 않았다면, `BABA`는 만들어지지 않습니다.

### Version 2.0

전체 이동 cycle은 크게 5개의 phase로 구성됩니다. 위쪽 `ICE`가 있는 문장이 1번, `LONELY BRICK (IS) TELE`가 15번입니다.

- phase 1(문장 1-2): 방향 지정 phase.(1.0과 동일)

- phase 2(문장 3-4): 처리 phase 1.(1.0과 동일)

- phase 3(문장 5-8): 2048 이동 phase. 2048의 룰대로 이동합니다. 이동하거나 합쳐지면 `HAND`가 생성됩니다.
  - 사실 Version 1.0는 2048의 이동을 완전하게 구현하지 못했습니다.
  - `2 2 4 8`과 같이 배치되어 있는 경우, 다음과 같은 과정을 거칩니다.
    - 왼쪽의 `2`가 `alt(2)`(`2*`로 표기)로 바뀝니다. `2* 2 4 8`
    - `2*`와 `2`가 합쳐집니다. `4* 4 8`
    - `4*`와 `4`가 합쳐집니다. `8* 8`
    - `8*`와 `8`이 합쳐집니다. `16*`
  - 하지만 실제 2048에서는 `4 4 8`의 결과가 나와야 합니다.

  - 따라서 *alt form*으로 바뀌는 조건을 수정했습니다.
  - **오브젝트가 바라보는 방향으로 합쳐질 일이 없는 경우 *alt form*으로 바뀝니다.**


- phase 4(문장 9-12): 랜덤 생성 setting phase. `TILE`에서 `BRICK`이 생성됩니다.
  - 이 방법은 `BRICK`의 방향을 좀 더 쉽게 다룰 수 있다는 장점이 있습니다.
  - 사실 이렇게 하게 된 이유는, `BABA NOT FACING ... IS ROSE`에서 오브젝트들이 `HEDGE`에 생성되어 있는 `BRICK`의 영향을 받아 정상적인 이동 처리가 안 되는 문제가 있었기 때문입니다.

- phase 5(문장 13-15): 랜덤 생성 phase.(1.0과 동일)

## Version 1.0
![2048 is you Version 1.0](/ver-1.0.png)
- `HEDGE`의 방향이 중요합니다. phase 4의 내용을 잘 읽어보시기 바랍니다. 나머지 오브젝트의 방향은 상관없습니다.
- 겹쳐진 부분은 (`HAND`, `FUNGUS`), (`STOP`, `DOWN`)입니다.

## Version 1.1
![2048 is you Version 1.1](/ver-1.1.png)
- 버그 수정: `FUNGUS`에서 만들어진 `BABA`가 `BELT`를 탈출하여 맵을 자유롭게 돌아다닐 수 있었던 부분을 수정
  - 개발자인 제가 정석대로 맵을 **클리어**하는 데 3시간이 걸렸지만, 이 버그를 이용하면 **5분(!)** 안에 깰 수 있습니다.

## Version 2.0
![2048 is you Version 2.0 - page 1](/ver-2.0-1.png)
![2048 is you Version 2.0 - page 2](/ver-2.0-2.png)
![2048 is you Version 2.0 - page 3](/ver-2.0-3.png)
- 대규모 수정: 이제 35×20 안에 맵이 들어갑니다!
  - 90개 정도 되는 문장을 지우고, 10개의 문장을 만들었습니다.
  - 이전 버전의 supermerge 문제를 해결했습니다.(위 설명 참고)
- 겹쳐진 오브젝트가 너무 많아서, 3개의 층을 따로 표시하기로 하였습니다.
- 1층의 `IS`와 `TILE`이 겹쳐진 부분은 둘 다 아래를 향하도록 해 주세요.
- 2층의 `TILE`들(잘 관찰해보세요!)은 모두 아래를 향해야 합니다.
