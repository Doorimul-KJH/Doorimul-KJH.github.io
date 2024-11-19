---
title: Adding Page View Tracking to a GitHub Pages Blog Using GoatCounter (Chirpy Theme)
author: Doorimul-KJH
date: 2024-11-17 00:00:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: false
---


# GoatCounter를 이용해 GitHub 블로그에 조회수 기능 추가하기

### Step 0. 사전 준비사항(개발환경)

- **macOS**: Apple Silicon M2 Max, **Sonoma 14.5.1**
- **Git 설치**: Homebrew 사용 권장
- **VS Code 설치**: 편집기
- **Github 가입**: GitHub 계정 생성 필요
- **기본 셸**: `zsh` 사용 (macOS 기본 셸 설정)

### GoatCounter를 이용해 GitHub 블로그에 조회수 기능 추가하기

**GoatCounter**는 오픈 소스 웹 분석 도구로, GitHub Pages 기반 블로그에 조회수 기능을 쉽게 추가할 수 있습니다.

- **GoatCounter 공식 웹사이트**: [https://www.goatcounter.com/](https://www.goatcounter.com/)

### Step 1. GoatCounter 가입 및 코드 생성 방법

1. **GoatCounter 웹사이트로 이동**: [GoatCounter 공식 웹사이트](https://www.goatcounter.com/)에 접속하세요.
2. **Sign up 버튼 클릭**: 우측 상단의 **Sign up** 버튼을 클릭하고 이메일 주소를 입력하여 계정을 생성하세요.

   <!-- ![GoatCounter 웹사이트에서 "Sign up" 버튼을 클릭하는 모습](https://doorimul-kjh.github.io/assets/img/posts/goatcounter_sign_up.png){: width="700" } -->

3. **사이트 등록**: 가입 후, 블로그에 연결할 사이트를 등록하세요. 사이트의 이름과 URL을 입력하고 **Create site** 버튼을 클릭하세요.

   <!-- ![사이트를 등록하는 화면](https://doorimul-kjh.github.io/assets/img/posts/) -->

4. **Tracking 코드 복사**: 사이트 생성이 완료되면 Tracking 코드를 제공합니다. 이 코드를 복사하세요.

   <!-- ![Tracking 코드를 복사하는 화면](https://doorimul-kjh.github.io/assets/img/posts/) -->

### Step 2. GoatCounter 설정

GoatCounter의 Tracking 코드를 블로그에 추가하여 조회수 기능을 활성화하세요.

1. **블로그 Repository로 이동**: GitHub에서 블로그로 사용할 Repository(예: `zoren.github.io`)로 이동하세요.
2. **Repository 설정 파일 수정**: `_config.yml` 또는 HTML 파일에서 GoatCounter 설정을 추가하세요.

   - `_config.yml` 파일의 **head** 섹션에 다음과 같이 Tracking 코드를 추가하세요.
   
   ```yaml
   head:
     custom: |
       <script data-goatcounter="https://your-goatcounter.goatcounter.com/count"
               async src="//gc.zgo.at/count.js"></script>
   ```
   - `data-goatcounter`의 URL 부분에는 GoatCounter에서 제공한 URL을 입력하세요.

   <!-- ![GoatCounter 설정을 `_config.yml` 파일에 추가하는 모습](https://doorimul-kjh.github.io/assets/img/posts/goatcounter_config.yml_modified.png){: width="700" } -->

3. **HTML 파일 수정**: HTML 파일의 `<head>` 태그 안에 Tracking 코드를 추가할 수도 있습니다. 일반적으로 기본 템플릿 파일에 추가하는 것이 좋습니다.

   ```html
   <script data-goatcounter="https://your-goatcounter.goatcounter.com/count"
           async src="//gc.zgo.at/count.js"></script>
   ```

### Step 3. 블로그 페이지에 GoatCounter 조회수 기능 확인

블로그 페이지에 접속한 후, 각 포스트의 조회수가 정상적으로 카운트되는지 확인하세요.

1. **GoatCounter 대시보드 접속**: [GoatCounter 대시보드](https://www.goatcounter.com/)에 접속하여 자신의 사이트를 선택하세요.
2. **조회수 확인**: 각 페이지의 조회수가 정상적으로 기록되는지 대시보드에서 확인하세요.

   <!-- ![GoatCounter 대시보드에서 조회수를 확인하는 모습](https://doorimul-kjh.github.io/assets/img/posts/goatcounter_dashboard.png){: width="700" } -->