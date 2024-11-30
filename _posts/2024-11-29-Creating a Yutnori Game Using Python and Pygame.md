---
title: Creating a Yutnori Game Using Python and Pygame
author: Doorimul-KJH
date: 2024-11-29 00:00:00 +0900
categories: [Blogging, Projects]
tags: [blogging]
pin: true
render_with_liquid: false
---


물론입니다. 모든 설명을 "~하였습니다." 형식으로 변경하여 정리해드리겠습니다.

---

# Pygame을 이용해 윷놀이 게임 만들기

### Step 0. 사전 준비사항(개발환경)

- **macOS**: Apple Silicon M2 Max, **Sonoma 14.5.1**
- **Git 설치**: Homebrew 사용 권장
- **VS Code 설치**: Python 코드 작성용 편집기
- **GitHub 계정**: 코드 공유 및 백업을 위해 GitHub 계정이 필요합니다.
- **기본 셸**: `zsh` 사용 (macOS 기본 셸 설정)

### Step 0-1. VS Code에 Python 환경 설정하기

1. **Python 설치**: Python 3.12 이상 버전을 사용합니다. [Python 공식 사이트](https://www.python.org/downloads/)에서 최신 버전을 다운로드하여 설치하였습니다.

2. **VS Code 확장 프로그램 설치**:
   - VS Code에서 **Python** 확장을 설치하였습니다. 이 확장은 Python 코드를 쉽게 작성하고 디버깅할 수 있게 해줍니다.
   - **Command Palette**를 열려면 `Cmd + Shift + P`를 누르고, "Python: Select Interpreter"를 선택하여 설치된 Python 3.12 버전을 선택하였습니다.

3. **Pygame 설치**:
   - **Pygame** 라이브러리는 터미널에서 아래 명령어로 설치하였습니다.

   ```bash
   pip install pygame==2.6.1
   ```

4. **설치 확인**:
   - Python 및 Pygame이 제대로 설치되었는지 확인하기 위해 터미널에 아래 명령어를 입력하였습니다.
   
   ```bash
   python --version
   pip show pygame
   ```

   Python 버전이 3.12 이상인지, Pygame 버전이 2.6.1인지 확인하였습니다.

### Step 1. 윷놀이 게임 설명

윷놀이는 한국 전통 보드 게임으로, 최소 2명에서 최대 4명까지 플레이할 수 있습니다. 각 플레이어는 4개의 말을 가지고 있으며, 목적은 모든 말을 상대보다 먼저 도착 지점까지 이동시키는 것입니다.

- **플레이어 수**: 최소 2명, 최대 4명.
- **말의 개수**: 각 플레이어당 4개의 말.
- **윷 던지기 규칙**:
  - 윷은 네 개의 막대로 이루어져 있으며, 결과에 따라 **도**, **개**, **걸**, **윷**, **모**가 나옵니다.
  - **모**나 **윷**이 나오면 플레이어는 한 번 더 윷을 던질 수 있습니다.
  - **상대 말을 잡으면** 플레이어는 추가로 한 번 더 윷을 던질 수 있습니다.
- **말 업기**: 같은 플레이어의 말들이 같은 위치에 있을 경우 함께 움직일 수 있습니다.
- **방향 선택**:
  - **양쪽 위 꼭짓점**과 **가운데 점**에서는 플레이어가 말의 이동 방향을 선택할 수 있습니다.
  - 이는 전략적으로 중요한 부분으로, 빠르게 이동하거나 상대 말을 잡기 위한 최적의 경로를 선택하는 데 도움이 됩니다.

### Step 2. 윷 던지기 함수 및 확률

윷 던지기는 4개의 윷가락을 던져 그 결과로 이동 거리를 결정합니다. 각 윷가락은 **평평한 면** 또는 **둥근 면**이 나올 수 있으며, 각 면이 나올 확률은 다릅니다.

아래 코드는 윷 던지기 기능을 구현한 것입니다:

```python
import random

# 윷 던지기 함수 정의
def throw_yoot():
    # 윷가락의 면 결정 (평평한 면 = True, 둥근 면 = False)
    # 평평한 면의 확률은 60%, 둥근 면의 확률은 40%로 설정되어 있습니다.
    sticks = [random.choices([True, False], weights=[60, 40])[0] for _ in range(4)]
    
    # 네 개의 윷가락 중 평평한 면이 나온 개수를 계산합니다.
    flat_count = sticks.count(True)

    if flat_count == 4:
        return "윷"  # 윷: 네 개 모두 평평한 면
    elif flat_count == 0:
        return "모"   # 모: 네 개 모두 둥근 면
    elif flat_count == 3:
        return "걸"  # 걸: 세 개가 평평한 면
    elif flat_count == 2:
        return "개"  # 개: 두 개가 평평한 면
    elif flat_count == 1:
        # 도: 한 개만 평평한 면일 때 1/4 확률로 백도
        if random.random() < 0.25:
            return "백도"
        else:
            return "도"
```

- **확률 설명**:
  - 윷놀이의 재미와 전통적인 확률을 반영하기 위해 **평평한 면**은 60%, **둥근 면**은 40%의 확률로 설정하였습니다. 
  - 이 설정은 실제 윷놀이의 물리적인 특성과 유사한 확률을 제공합니다. 평평한 면이 더 많이 나올 확률이 높은 이유는 실제 윷놀이에서 평평한 면이 더 무거워 바닥에 깔리기 쉽기 때문입니다.
  - **윷 (4개 평평한 면)**: 네 개의 윷가락이 모두 평평한 면이 나올 확률입니다.
  - **모 (4개 둥근 면)**: 네 개의 윷가락이 모두 둥근 면이 나올 확률입니다.
  - **걸, 개, 도**는 각각 평평한 면의 개수에 따라 결정됩니다.
  - **백도**는 도의 특별한 경우로, 1/4 확률로 뒤로 한 칸 가는 결과가 나옵니다. 이는 게임의 변수 요소로, 예기치 않은 반전을 제공하여 게임의 긴장감을 높였습니다.

### Step 3. 윷판 그리는 방법

윷판은 게임의 주요 구성 요소로, 플레이어들이 말을 이동하는 경로를 시각적으로 표시해줍니다. 아래 코드는 윷판을 Pygame으로 그리는 방법을 설명하겠습니다.

```python
# 윷 판 테두리 생성
for i in range(1, 21):
    if i < 6:
        ypos -= button_interval  # 상단에서 위쪽 방향으로 이동
    elif i < 11:
        xpos -= button_interval  # 왼쪽으로 이동
    elif i < 16:
        ypos += button_interval  # 하단으로 이동
    else:
        xpos += button_interval  # 오른쪽으로 이동

    # 특정 위치에 큰 원 표시 (윷판의 꼭짓점 및 시작점)
    if i == 5 or i == 10 or i == 15:
        img = bigcircle_img
    elif i == 20:
        img = startcircle_img
    else:
        img = circle_img

    # 버튼의 위치와 이미지 정의
    button_rect = img.get_rect(center=(xpos, ypos))
    pan_buttons.append((img, button_rect))
    board_pos.append((xpos, ypos))

# 첫 번째 대각선 그리기
xpos = button_size_x * 7 - 10
ypos = button_size_y - 10
for p in range(6):
    if p == 3:
        xpos -= button_size_x
        ypos += button_size_y
    else:
        if p == 0:
            xpos -= button_size_x
            ypos += button_size_y
        else:
            button_rect = circle_img.get_rect(center=(xpos, ypos))
            pan_buttons.append((circle_img, button_rect))
            board_pos.append((xpos, ypos))

            xpos -= button_size_x
            ypos += button_size_y

# 두 번째 대각선 그리기
xpos = button_size_x - 10
ypos = button_size_y - 10
for p in range(6):
    if p == 0:
        xpos += button_size_x
        ypos += button_size_y
    else:
        if p == 3:
            img = bigcircle_img
        else:
            img = circle_img

        button_rect = img.get_rect(center=(xpos, ypos))
        pan_buttons.append((img, button_rect))
        board_pos.append((xpos, ypos))

        xpos += button_size_x
        ypos += button_size_y
```

- **코드 설명**:
  - **테두리 생성**:
    - `for i in range(1, 21)` 반복문은 윷판의 테두리를 순서대로 그리기 위한 것입니다.
    - 각 반복에서 `xpos`와 `ypos`를 조정하여 상단, 좌측, 하단, 우측으로 이동하면서 윷판의 테두리를 그렸습니다.
    - 특정 위치(`i == 5`, `i == 10`, `i == 15`, `i == 20`)에서는 **특수 이미지** (`bigcircle_img` 또는 `startcircle_img`)를 사용하여 시작점과 주요 지점을 표시하였습니다.
  
  - **대각선 경로 추가**:
    - 첫 번째 대각선과 두 번째 대각선을 그리기 위해 각각 별도의 반복문을 사용하였습니다.
    - 대각선 경로는 윷놀이에서 특수한 이동 경로로, 이를 통해 플레이어들이 지름길을 선택하여 빠르게 도착 지점으로 이동할 수 있도록 하였습니다.
    - 각 대각선의 중간 지점(`p == 3`)에는 큰 원(`bigcircle_img`)을 사용하여 방향 변경 지점을 강조하였습니다.

위의 코드를 통해 전통적인 윷판의 모양을 Pygame으로 구현하고, 말들이 이동할 경로를 설정하였습니다. 테두리와 대각선 경로를 명확히 정의하여 플레이어들이 이동할 수 있는 각 위치를 직관적으로 나타내도록 설계하였습니다.