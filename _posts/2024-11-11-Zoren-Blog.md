---
title: How to build My Own Blog Using GitHub Pages (Chirpy Theme)
author: Zoren
date: 2024-11-11 00:00:00 +0900
categories: [Blogging, Tutorial]
tags: [writing]
render_with_liquid: false
---

Apple Silicon 환경에서 GitHub Pages를 이용해 Jekyll 기반의 블로그를 만드는 방법입니다.

### Step 0. 사전 준비사항

- macOS (Apple Silicon M2 Max) Sonoma 14.5.1
- Git 설치 (Homebrew 사용 권장)
- VS Code 설치
- Github 가입
- 기본 셸은 `zsh` 사용 (macOS 기본 셸 설정)

### Step 1. Github Page 생성

#### Step 1-1. Repository 생성

- **github username**이 zoren인 경우, **repository name**을 zoren.github.io로 설정하세요.
- **Public** 체크하세요.
- **Add a README file** 체크하세요.

#### Step 1-2. Github Page 설정

- 생성한 Repository로 이동하여, 상단 **Settings** 클릭하세요.
- 좌측 **Pages** 클릭하세요.
- **Source**를 Deploy from a branch로 설정하세요.
- 사이트 접속을 확인하세요 (예시: https://zoren.github.io).

#### Step 1-3. VS Code 활용

##### Repository Clone
- VS Code 열기 >> F1 키 입력 >> git clone 검색 >> Git: Clone 선택하세요.
- Repository 주소 입력 >> Clone할 위치 선택하세요.

##### 로컬 변경사항 적용
- Clone한 Repository 열기 (README.md 파일 확인)
- 좌측 **Source Control** 메뉴 선택하세요.
- `+` 버튼을 클릭하여 변경사항 추가하세요.
- Commit 메시지 입력, Commit & Push 하세요.
- 사이트 반영을 확인하세요 (예시: https://zoren.github.io).

### Step 2. 로컬 개발 환경 구축

#### Step 2-1. rbenv와 Ruby 설치

- rbenv와 ruby-build를 설치하세요:
  ```bash
  brew install rbenv ruby-build
  ```
- rbenv 초기화 및 설정하세요:
  ```bash
  rbenv init
  ```
- 셸 설정 파일(`~/.zshrc` 또는 `~/.bash_profile`)에 아래 내용을 추가하여 rbenv를 활성화하세요:
  ```bash
  echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.zshrc
  echo 'eval "$(rbenv init -)"' >> ~/.zshrc
  source ~/.zshrc
  ```
- PATH 설정을 확인하세요:
  ```bash
  echo $PATH
  ```
- Ruby를 설치하세요(매우 중요) (예: 3.3.5 버전):
  ```bash
  #brew install ruby 했다가, 처음부터 다시하는 낭패를 보았습니다...
  rbenv install 3.3.5
  ```
- Ruby 버전을 확인하세요:
  ```bash
  ruby -v
  ```

#### Step 2-2. Jekyll 및 Bundler 설치

- Jekyll과 Bundler를 설치하세요:
  ```bash
  gem install jekyll bundler
  ```
- 설치를 확인하세요:
  ```bash
  jekyll -v
  bundler -v
  ```

### Step 3. Jekyll 블로그 생성 및 서버 구축

#### Step 3-1. Jekyll 블로그 생성

- Jekyll 블로그를 생성할 디렉토리로 이동 후, 아래 명령어를 실행하세요:
  ```bash
  jekyll new .
  ```

#### Step 3-2. Jekyll 서버 실행

- 필요한 Gem을 설치하세요:
  ```bash
  bundle install
  ```
- Jekyll 서버를 실행하세요:
  ```bash
  bundle exec jekyll serve
  ```
- 브라우저에서 `http://127.0.0.1:4000` 또는 `http://localhost:4000`을 열어 로컬에서 블로그를 미리 보세요.

### Step 4. Git 설정 및 GitHub에 푸시

#### Step 4-1. Git 초기화 및 GitHub 연결

- Git을 초기화하세요:
  ```bash
  git init
  ```
- GitHub 원격 저장소를 연결하세요:
  ```bash
  git remote add origin https://github.com/zoren/zoren.github.io.git
  ```
- 모든 파일을 스테이징하고 Commit 하세요:
  ```bash
  git add .
  git commit -m "Commit Message"
  ```
- 파일을 GitHub로 Push 하세요:
  ```bash
  git branch -M main
  git push -u origin main
  ```

### Step 5. GitHub Pages 설정

- GitHub 저장소 설정(Settings)에서 **Pages** 섹션으로 이동하세요.
- **Source**를 `Deploy from a branch`로 설정하고 저장하여 블로그를 활성화하세요.
- 사이트 접속을 확인하세요 (예시: https://zoren.github.io).

### Step 6. Jekyll 테마 적용

#### Step 6-0. 테마 선택

- [Jekyll 테마 사이트](http://jekyllthemes.org), [GitHub Topics](https://github.com/topics/jekyll-theme) 등에서 원하는 테마를 선택하세요.

#### Step 6-1. Chirpy 테마 적용

- [Chirpy 테마 GitHub 페이지](https://github.com/cotes2020/jekyll-theme-chirpy)에서 테마 파일을 다운로드하세요.
- 압축 해제 후 모든 파일과 폴더를 로컬 리포지토리로 복사하세요.
- 필요한 Gem을 설치하세요:
  ```bash
  bundle install
  ```
- Jekyll 서버를 실행하세요:
  ```bash
  bundle exec jekyll serve
  ```
- 로컬에서 변경 사항을 확인한 후 GitHub에 Commit 및 Push 하세요:
  ```bash
  git add .
  git commit -m "Chirpy 테마 적용"
  git push origin main
  ```

### Step 7. GitHub Actions 설정 (자동 배포)

- 원격 Repository의 상단 **Settings** 클릭하세요.
- 좌측 **Pages** 클릭하세요.
- **Source**를 `GitHub Actions`로 설정하세요.
- `Configure` 클릭 후 기본 설정 파일을 사용하여 **Commit changes** 클릭하세요.
- 로컬 Repository에서 pull 명령어로 최신 변경 사항을 반영하세요:
  ```bash
  git pull origin main
  ```

### Step 8. 테마 상세 설정

#### Step 8-1. `.gitignore` 파일 수정

- `.gitignore` 파일에 다음 내용을 추가하여 빌드 파일을 제외하세요:
  ```
  # Misc
  _sass/dist
  assets/js/dist
  ```

#### Step 8-2. `_config.yml` 파일 수정(중요)

- `_config.yml` 파일을 열고 사이트 설정을 수정하세요:
  ```yml
  timezone: Asia/Seoul
  url: "https://zoren.github.io"
  github:
    username: zoren
  ```
- 변경 사항을 Commit 및 Push 하세요:
  ```bash
  git add .
  git commit -m "Update _config.yml"
  git push origin main
  ```


