---
title: Using "Utterance" to Enable Comments on a GitHub Pages Blog (Chirpy Theme)
author: Doorimul-KJH
date: 2024-11-17 00:00:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: false
---


# Utterance를 이용해 GitHub 블로그에 댓글 기능 추가하기

### Step 0. 사전 준비사항(개발환경)

- **macOS**: Apple Silicon M2 Max, **Sonoma 14.5.1**
- **Git 설치**: Homebrew 사용 권장
- **VS Code 설치**: 편집기
- **Github 가입**: GitHub 계정 생성 필요
- **기본 셸**: `zsh` 사용 (macOS 기본 셸 설정)

### Utterance 앱 설치

**Utterance**는 GitHub Issues를 활용하여 댓글을 추가할 수 있는 오픈소스 프로젝트입니다. 이를 사용하면 GitHub Pages 기반 블로그에 손쉽게 댓글 기능을 추가할 수 있습니다.

- **Utterance GitHub 앱 주소**: [https://github.com/apps/utterances](https://github.com/apps/utterances)

### Step 1. Utterance 가입 및 설치 방법

1. **Utterance 설치 페이지로 이동**: [Utterance GitHub 앱 링크](https://github.com/apps/utterances)
2. **Install 버튼 클릭**: GitHub 페이지에서 Install 버튼을 찾아 클릭하세요.

  - ![페이지에서 Utterance 앱을 설치하기 위해 "Install" 버튼을 누르는 모습](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_install_button.png)
  - ![페이지에서 Utterance 앱을 설치하기 위해 "Install" 버튼을 누르는 모습](/assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_install_button.png)
  - ![페이지에서 Utterance 앱을 설치하기 위해 "Install" 버튼을 누르는 모습](https://github.com/Doorimul-KJH/Doorimul-KJH.github.io/blob/main/assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_blog_issue_mapping.png)
   <!-- <GitHub 페이지에서 Utterance 앱을 설치하기 위해 "Install" 버튼을 누르는 모습> -->

3. **Repository 선택**: Utterances를 설치할 Repository를 선택하세요.
   - 블로그로 사용할 Repository (예: `zoren.github.io`)를 선택하세요.
   - **Only select repositories**를 선택하여 필요한 Repository만 추가하도록 설정할 수 있습니다.

   ![Utterances를 설치할 Repository를 선택하는 화면](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_select_repository.png)
   <!-- <Utterances를 설치할 Repository를 선택하는 화면> -->

4. **Install 클릭**: 선택이 완료되면 하단의 **Install** 버튼을 눌러 설치를 진행하세요.

5. **Utterance와 연결할 Repo 작성하기**: Utterance와 연결할 Repository 이름을 정확히 작성하세요. 이는 댓글이 저장될 공간입니다.
  - ![Utterance와 연결할 Repo 작성하기](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_configuration_repository.png)

6. **Issue Mapping 방법 선택하기**: 댓글이 어떻게 매핑될지 선택하세요. 일반적으로 pathname, url, title 등으로 설정할 수 있습니다. 저 같은 경우에는 가장 첫 번째인 pathname을 선택했습니다. 이 옵션은 각 블로그 포스트의 URL 경로를 기준으로 댓글이 매핑되므로 편리합니다.

  - ![Blog와 Issue를 어떻게 Mapping 할 것 인지 선택하는 화면](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_blog_issue_mapping.png)


### Step 2. Utterance 설정

Utterances를 설치한 후, 블로그에서 댓글 기능을 활성화하기 위해 필요한 설정을 추가하세요.

1. **블로그 Repository로 이동**: GitHub에서 블로그로 사용할 Repository(예: `zoren.github.io`)로 이동하세요.
2. **Repository 설정 파일 수정**: `_config.yml` 또는 원하는 페이지에서 Utterance 설정을 추가하세요.
3. **HTML 파일 수정**:
   - 블로그 포스트의 끝에 Utterance 스크립트를 추가하세요. 일반적으로 포스트 템플릿 파일에 추가하는 것이 좋습니다.
   ```html
   <script src="https://utteranc.es/client.js"
           repo="zoren/zoren.github.io"
           issue-term="pathname"
           theme="github-light"
           crossorigin="anonymous"
           async>
   </script>
   ```
   - `repo`는 댓글을 저장할 Repository 이름입니다.
   - `issue-term`는 댓글이 저장되는 기준을 설정하는 것으로, `pathname`을 사용하면 URL 경로를 기준으로 Issue가 생성됩니다.

  ![Utterance 설정을 `_config.yml` 또는 HTML 파일에 추가하는 모습](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_config.yml_modified.png)

   <!-- <Utterance 설정을 `_config.yml` 또는 HTML 파일에 추가하는 모습> -->

4. **Utterance와 연결할 Repo 작성하기**: Utterance와 연결할 Repository 이름을 정확히 작성하세요. 이는 댓글이 저장될 공간입니다.

5. **Issue Mapping 방법 선택하기**: 댓글이 어떻게 매핑될지 선택하세요. 일반적으로 `pathname`, `url`, `title` 등으로 설정할 수 있습니다. `pathname`을 선택하면 각 블로그 포스트의 URL 경로를 기준으로 댓글이 매핑됩니다.

6. **Issue label 작성하기**: 댓글로 생성된 Issue에 특정 라벨을 추가하여 관리할 수 있습니다. 예를 들어 `blog-comment`와 같은 라벨을 추가해보세요.

### Step 3. 블로그 페이지에 Utterance 댓글 기능 확인

블로그 페이지에 접속한 후, 각 포스트 하단에서 Utterance 댓글 기능이 올바르게 표시되는지 확인하세요.

1. **포스트 하단에 댓글 기능 표시 확인**: 포스트를 열면 하단에 Utterance 댓글 영역이 표시됩니다. 댓글을 달기 위해 GitHub 계정으로 로그인할 수 있습니다.

   

2. **댓글 작성 테스트**: GitHub 계정으로 로그인 후, 댓글을 달아보세요. 댓글이 정상적으로 저장되고 표시되는지 확인하세요.

   ![GitHub 계정으로 로그인하고 댓글을 작성하는 모습](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_comment_test.png)
   <!-- <GitHub 계정으로 로그인하고 댓글을 작성하는 모습> -->

### Step 4. Utterance 작동 확인

Utterances를 설정한 후, 실제로 블로그에 댓글 기능이 어떻게 작동하는지 확인해보세요.

1. **댓글 작성 및 GitHub Issues 생성 확인**:
   - 블로그 포스트에서 댓글을 작성하면, 해당 Repository의 **Issues** 탭에서 작성된 댓글이 Issue로 자동 생성됩니다.

   ![GitHub Repository의 Issues 탭에 댓글이 Issue로 생성된 모습](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_comment_check.png)
   <!-- <블로그 포스트 하단에 Utterance 댓글 기능이 나타나는 모습> -->
   <!-- <GitHub Repository의 Issues 탭에 댓글이 Issue로 생성된 모습> -->

2. **댓글 관리 방법**:
   - GitHub Issues에서 댓글을 직접 관리할 수 있습니다. 예를 들어, 부적절한 댓글을 삭제하거나, 스팸으로 표시할 수 있습니다.

   ![GitHub Issues에서 댓글을 관리하는 모습](../assets/img/Using_Utterance_to_Enable_Comments_on_a_GitHub_Pages_Blog_(Chirpy_Theme)/utterance_comment_function.png)
   <GitHub Issues에서 댓글을 관리하는 모습>
