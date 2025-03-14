---
title: Using "Utterance" to Enable Comments on a GitHub Pages Blog (Chirpy Theme)
author: Doorimul-KJH
date: 2024-11-17 00:00:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
pin: true
render_with_liquid: false
---


# Utterance를 이용해 GitHub 블로그에 댓글 기능 추가하기

### Step 0. 사전 준비사항(개발환경)

- **VS Code 설치**: 편집기
- **Github 가입**: GitHub 계정 생성 필요

### Utterance 앱 설치

**Utterance**는 GitHub Issues를 활용하여 댓글을 추가할 수 있는 오픈소스 프로젝트입니다. 이를 사용하면 GitHub Pages 기반 블로그에 손쉽게 댓글 기능을 추가할 수 있습니다.

- **Utterance GitHub 앱 주소**: [https://github.com/apps/utterances](https://github.com/apps/utterances)

### Step 1. Utterance 가입 및 설치 방법

1. **Utterance 설치 페이지로 이동**: [Utterance GitHub 앱 링크](https://github.com/apps/utterances)

2. **Install 버튼 클릭**: GitHub 페이지에서 Install 버튼을 찾아 클릭하세요.

   ![Install 버튼을 클릭하여 Utterance 설치](https://doorimul-kjh.github.io/assets/img/posts/utterance_install_button.png){: width="700" }

3. **Repository 선택**: Utterances를 설치할 Repository를 선택하세요.
   - 블로그로 사용할 Repository (예: `zoren.github.io`)를 선택하세요.
   - **Only select repositories**를 선택하여 필요한 Repository만 추가하도록 설정할 수 있습니다.
   ![Repository 선택 화면](https://doorimul-kjh.github.io/assets/img/posts/utterance_select_repository.png){: width="700"}

4. **Install 클릭**: 선택이 완료되면 하단의 **Install** 버튼을 눌러 설치를 진행하세요.
   <!-- ![Install 버튼으로 설치 진행](https://doorimul-kjh.github.io/assets/img/posts/utterance_){: width="700"} -->

5. **Utterance와 연결할 Repo 작성하기**: Utterance와 연결할 Repository 이름을 정확히 작성하세요. 이는 댓글이 저장될 공간입니다.
   <!-- ![Utterance와 연결할 Repo 작성하기](https://doorimul-kjh.github.io/assets/img/posts/utterance_){: width="700"} -->

6. **Issue Mapping 방법 선택하기**: 댓글이 어떻게 매핑될지 선택하세요. 일반적으로 pathname, url, title 등으로 설정할 수 있습니다. 저 같은 경우에는 가장 첫 번째인 pathname을 선택했습니다. 이 옵션은 각 블로그 포스트의 URL 경로를 기준으로 댓글이 매핑되므로 편리합니다.
   ![Issue Mapping 방법 선택](https://doorimul-kjh.github.io/assets/img/posts/utterance_blog_issue_mapping.png){: width="700"}

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
   
   ![HTML 파일에 Utterance 설정 추가](https://doorimul-kjh.github.io/assets/img/posts/utterance_config.yml_modified.png){: width="700" }

4. **Utterance와 연결할 Repo 작성하기**: Utterance와 연결할 Repository 이름을 정확히 작성하세요. 이는 댓글이 저장될 공간입니다.

5. **Issue Mapping 방법 선택하기**: 댓글이 어떻게 매핑될지 선택하세요. 일반적으로 `pathname`, `url`, `title` 등으로 설정할 수 있습니다. `pathname`을 선택하면 각 블로그 포스트의 URL 경로를 기준으로 댓글이 매핑됩니다.

### Step 3. 블로그 페이지에 Utterance 댓글 기능 확인

블로그 페이지에 접속한 후, 각 포스트 하단에서 Utterance 댓글 기능이 올바르게 표시되는지 확인하세요.

1. **포스트 하단에 댓글 기능 표시 확인**: 포스트를 열면 하단에 Utterance 댓글 영역이 표시됩니다. 댓글을 달기 위해 GitHub 계정으로 로그인할 수 있습니다.
   <!-- ![블로그 포스트 하단에 댓글 기능 표시 확인](https://doorimul-kjh.github.io/assets/img/posts/utterance_comment_test.png){: width="700" } -->

2. **댓글 작성 테스트**: GitHub 계정으로 로그인 후, 댓글을 달아보세요. 댓글이 정상적으로 저장되고 표시되는지 확인하세요.
   ![GitHub 계정으로 댓글 작성 테스트](https://doorimul-kjh.github.io/assets/img/posts/utterance_comment_test.png){: width="700" }

### Step 4. Utterance 작동 확인

Utterances를 설정한 후, 실제로 블로그에 댓글 기능이 어떻게 작동하는지 확인해보세요.

1. **댓글 작성 및 GitHub Issues 생성 확인**:
   - 블로그 포스트에서 댓글을 작성하면, 해당 Repository의 **Issues** 탭에서 작성된 댓글이 Issue로 자동 생성됩니다.
   ![GitHub Issues에 댓글 자동 생성 확인](https://doorimul-kjh.github.io/assets/img/posts/utterance_comment_check.png){: width="700" }

2. **댓글 관리 방법**:
   - GitHub Issues에서 댓글을 직접 관리할 수 있습니다. 예를 들어, 부적절한 댓글을 삭제하거나, 스팸으로 표시할 수 있습니다.
   ![GitHub Issues에서 댓글 관리 화면](https://doorimul-kjh.github.io/assets/img/posts/utterance_comment_function.png){: width="700" }
