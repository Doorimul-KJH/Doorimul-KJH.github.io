---
title: Creating a Yutnori Game Using Python and Pygame
author: Doorimul-KJH
date: 2024-11-29 00:00:00 +0900
categories: [Blogging, Projects]
tags: [blogging]
pin: true
# image: https://doorimul-kjh.github.io/assets/
render_with_liquid: false
---

# Pygame을 이용해 윷놀이 게임 만들기
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-icon.png)

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
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-board.png)

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


### Step 4. 윷놀이에 필요한 이미지 로드

윷놀이를 구현하기 위해서는 다양한 이미지가 필요합니다. 아래 코드는 게임에 필요한 이미지를 Pygame으로 로드하는 과정에 대해 설명하겠습니다.

사용된 이미지
- ![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-bigcircle.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-bigblue.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-bigred.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-biggreen.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-bigyellow.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-bigorange.png)
- ![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-circle.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-blue.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-red.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-yellow.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-green.jpg)
![이미지](https://doorimul-kjh.github.io/assets/img/posts/yootnori-orange.png)

```python
circle_img = pygame.image.load(os.path.join(img_path, "circle.jpg"))
bigcircle_img = pygame.image.load(os.path.join(img_path, "bigcircle.jpg"))
startcircle_img = pygame.image.load(os.path.join(img_path, "startcircle.jpg"))
line_img = pygame.image.load(os.path.join(img_path, "line.png"))

bigorange_img = pygame.image.load(os.path.join(img_path, "bigorange.png"))
orange_img = pygame.image.load(os.path.join(img_path, "orange.png"))

# 말 이미지 로드 (파란색, 초록색, 빨간색, 노란색)
bigblue_img = [
    pygame.image.load(os.path.join(img_path, f"bigblue{i}.jpg")) for i in range(1, 5)
]
blue_img = [
    pygame.image.load(os.path.join(img_path, f"blue{i}.jpg")) for i in range(1, 5)
]

biggreen_img = [
    pygame.image.load(os.path.join(img_path, f"biggreen{i}.jpg")) for i in range(1, 5)
]
green_img = [
    pygame.image.load(os.path.join(img_path, f"green{i}.jpg")) for i in range(1, 5)
]

bigred_img = [
    pygame.image.load(os.path.join(img_path, f"bigred{i}.jpg")) for i in range(1, 5)
]
red_img = [
    pygame.image.load(os.path.join(img_path, f"red{i}.jpg")) for i in range(1, 5)
]

bigyellow_img = [
    pygame.image.load(os.path.join(img_path, f"bigyellow{i}.jpg")) for i in range(1, 5)
]
yellow_img = [
    pygame.image.load(os.path.join(img_path, f"yellow{i}.jpg")) for i in range(1, 5)
]

select_big_img = pygame.image.load(os.path.join(img_path, "bigorange.png"))
select_img = pygame.image.load(os.path.join(img_path, "orange.png"))
```

**설명**:

- **기본 이미지 로드**: 
  - `circle_img`, `bigcircle_img`, `startcircle_img`, `line_img` 등은 윷판에서 사용하는 기본적인 원과 라인의 이미지로, 윷놀이 판에 표시되는 각기 다른 지점들을 시각적으로 구분하기 위해 사용하였습니다.
- **색깔별 말 이미지 로드**:
  - 파란색(`blue_img`), 초록색(`green_img`), 빨간색(`red_img`), 노란색(`yellow_img`) 말 이미지를 각 플레이어에게 할당하였습니다. 각 플레이어의 말은 작은 말(`blue_img`, `green_img`, 등)과 큰 말(`bigblue_img`, `biggreen_img`, 등)으로 나뉘어져 있으며, 큰 말은 특정 조건에서 업힐 때 사용되었습니다.
- **리스트 활용**:
  - 각 말의 이미지는 리스트로 관리하여, 각각의 플레이어가 사용하는 4개의 말을 쉽게 접근하고 관리할 수 있도록 하였습니다. 이 방식을 사용하면 코드의 반복을 줄이고, 말의 상태를 쉽게 업데이트할 수 있습니다.

### Step 5. 윷놀이에 필요한 버튼 생성

게임 플레이를 위해 다양한 버튼이 필요합니다. 아래 코드는 윷놀이에 필요한 버튼을 생성하는 방법에 대해 설명하겠습니다.

```python
# 윷 던지기 버튼 생성
throw_button_rect = pygame.Rect(button_size_x * 9, button_size_y * 10, button_size_x * 3, button_size_y)
throw_button_color = YELLOW

# 윷 던지기 선택 효과 생성
throw_new_rect = pygame.Rect(button_size_x * 9, button_size_y * 10, button_size_x * 3, button_size_y)
throw_new_color = ORANGE

# 새로운 말 버튼 생성
new_piece_button_rect = pygame.Rect(button_size_x * 9, button_size_y * 11, button_size_x * 3, button_size_y)
new_piece_button_color = LIGHT_BLUE

# 새로운 말 선택 효과 생성
select_new_button_rect = pygame.Rect(button_size_x * 9, button_size_y * 11, button_size_x * 3, button_size_y)
select_new_button_color = ORANGE

# 윷 판 클릭 버튼 생성
pan_click_button_rect = pygame.Rect(button_size_x * 9, button_size_y * 9, button_size_x * 3, button_size_y)
pan_click_button_color = LIGHT_BLUE
```

**설명**:

- **버튼 생성 방법**:
  - `pygame.Rect()`를 사용하여 버튼의 위치와 크기를 지정하였습니다. 예를 들어 `throw_button_rect`는 윷 던지기 버튼으로, `(button_size_x * 9, button_size_y * 10, button_size_x * 3, button_size_y)`의 좌표와 크기를 가지도록 설정하였습니다.
- **색상 설정**:
  - 각 버튼에는 특정한 색상을 부여하여 시각적인 구분을 명확히 하였습니다. 예를 들어, 윷 던지기 버튼에는 **노란색(YELLOW)**이 사용되었습니다. 이는 사용자가 쉽게 인지할 수 있도록 시각적으로 구분된 색상을 부여한 것입니다.
- **선택 효과 버튼 생성**:
  - 버튼을 선택했을 때의 효과를 제공하기 위해 선택 효과 버튼(`throw_new_rect`, `select_new_button_rect`)도 별도로 생성하였습니다. 이를 통해 버튼이 선택된 상태와 그렇지 않은 상태를 쉽게 구분할 수 있도록 하였습니다.
- **새로운 말 버튼**:
  - 새로운 말을 게임에 추가할 수 있는 버튼(`new_piece_button_rect`)을 생성하였습니다. 이 버튼은 사용자가 윷을 던진 후 새로운 말을 판에 올릴 때 사용하는 버튼입니다.
  
### Step 6. 윷놀이 게임 알고리즘

윷놀이의 말 이동을 위한 게임 알고리즘은 복잡한 윷판의 경로를 구현하기 위해 여러 가지 방식으로 구현할 수도 있습니다. 여기서는 **이중 연결 리스트**와 **딕셔너리**를 이용한 두 가지 방법을 소개하겠습니다.

#### 첫 번째 방법: 이중 연결 리스트로 구현

- **윷판의 경로를 이중 연결 리스트로 구현**하는 방법이 있습니다. 각 노드는 윷놀이 판의 특정 위치를 나타내며, 각 노드는 전 노드와 다음 노드를 가리키는 포인터를 가지도록 해야합니다.
- **말 이동**:
  - 윷을 던진 후 나온 값만큼 현재 노드에서부터 하나씩 이동하도록 구현하는 것이 좋습니다.
  - 예를 들어, 주사위 값이 3이라면 현재 노드에서부터 1씩 주사위 값을 소모하며 이동하는 것입니다.
- **특수한 노드 처리**:
  - 노드 번호가 5, 10, 22일 경우 특수한 이동 경로가 존재하여, 남은 주사위 값이 1 이상일 때 다음 특수한 노드로 이동하게 구현하는 것이 좋습니다.
  - 주사위 값이 **-1(백도)**인 경우, 현재 위치에서 이전 노드로 이동하였습니다. 단, 왼쪽 하단 꼭짓점의 경우 말이 어느 방향으로부터 왔는지 확인해야합니다.
- **shortway 값**:
  - 룰에 따라 각 말의 `shortway` 값을 사용하여 특수한 경로를 선택할 수 있도록 구현해야할 수도 있습니다. 자동으로 지름길로 말이 이동할 것인지, 플레이어가 선택하게 할 것인지 고려해야합니다. 만약, 플레이어가 선택할 수 있도록 한다면 말의 속성 값 중 `shortway`를 변경해야합니다. (변수 명은 자유)

#### 두 번째 방법: 딕셔너리로 구현(이O윤 교수님 아이디어)

- **딕셔너리 사용**:
  - 윷판의 각 노드를 딕셔너리로 표현하고, 해당 노드에서 나올 수 있는 모든 결과를 딕셔너리에 저장할 수 있습니다.
  - 예를 들어, **3번 노드**에서 **개**가 나온 경우, 딕셔너리는 `{3번 노드: {도: 4, 개: 5, 걸: 6, 윷: 7, 모: 8}}` 형태로 정의할 수 있습니다.
  - 이러한 방식을 채택하면 모든 경우의 수를 미리 정의하여 윷놀이의 특수항 경우 까지 간단하게 처리할 수 있는 장점이 있습니다.
- **특수한 경우 처리**:
  - **꼭짓점**과 **중심점**과 같은 특수한 위치에서의 이동을 미리 정의하여, 사용자가 특수한 위치에 도달했을 때의 이동 경로를 딕셔너리를 통해 즉시 확인할 수 있도록 하였습니다.