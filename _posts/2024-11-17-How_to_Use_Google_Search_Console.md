---
title: Using Google Search Console to Expose Your GitHub Pages Blog to Search Engines
author: Doorimul-KJH
date: 2024-11-17 00:00:00 +0900
categories: [Blogging, Tutorial]
tags: [SEO, Google, blogging]
render_with_liquid: false
---


# Google Search Console을 이용해 GitHub 블로그를 검색 엔진에 노출시키기

### Step 0. 사전 준비사항(개발환경)

- **VS Code 설치**: 편집기
- **Github 가입**: GitHub 계정 생성 필요

### Google Search Console을 이용해 GitHub 블로그를 검색 엔진에 노출시키기

**Google Search Console**은 블로그를 Google 검색에 노출시키기 위해 사용하는 도구입니다. 이를 통해 사이트의 검색 성능을 모니터링하고 최적화할 수 있습니다.

- **Google Search Console 웹사이트**: [https://search.google.com/search-console](https://search.google.com/search-console)

### Step 1. Google Search Console에 사이트 등록

1. **Google Search Console 접속**: [Google Search Console](https://search.google.com/search-console)에 접속하세요.

2. **시작하기 버튼 클릭**:  **시작하기** 버튼을 클릭하세요.
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_start.png){: width="700" }

3. **사이트 등록**: 도메인 또는 URL Prefix를 입력하여 사이트를 등록하세요. 
   - **왼쪽에서 URL Prefix 선택 후 블로그 주소 입력**: 일반적으로 `URL Prefix`를 선택하고 자신의 블로그 주소를 입력하세요 (예: `https://zoren.github.io`).
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_url.png){: width="700" }

4. **소유권 확인(중요)**: 소유권을 확인하는 단계에서 HTML 파일 업로드, HTML 태그 추가, Google Analytics 계정 연결 등의 방법이 제공됩니다. 
   - **HTML 태그 추가 선택**: 여러 옵션 중에서 **HTML 태그 추가**를 선택하고, 제공된 메타 태그를 복사하세요.
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_html.png){: width="700" }

### Step 2. 소유권 확인을 위한 HTML 태그 추가

1. **블로그 Repository로 이동**: GitHub에서 블로그로 사용할 Repository(예: `zoren.github.io`)로 이동하세요.
2. **HTML 파일 수정(중요)**: `_config.yml` 또는 블로그의 기본 HTML 파일에 Google 소유권 확인을 위한 메타 태그를 추가하세요. 
   - **템플릿 파일의 `<head>` 태그에 추가**: 일반적으로 기본 템플릿 파일의 `<head>` 태그에 추가합니다.
      ```html
      <meta name="google-site-verification" content="YOUR_VERIFICATION_CODE" />
      ```
   - `YOUR_VERIFICATION_CODE` 부분에 Google Search Console에서 제공된 코드를 입력하세요.

### Step 3. 사이트맵 추가하기

1. **사이트맵 파일 생성(필수)**: 블로그의 루트 디렉토리에 `sitemap.xml` 파일을 생성하세요. 아래는 `sitemap.xml`의 예시입니다.

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9">
     <url>
       <loc>https://zoren.github.io/posts/sample-post/</loc>
       <lastmod>2024-11-17T00:00:00+09:00</lastmod>
       <changefreq>weekly</changefreq>
       <priority>0.7</priority>
     </url>
   </urlset>
   ```

   - **`loc` 태그에 URL 입력**: `loc` 태그에는 블로그 포스트의 URL을 입력하세요.
   - **`lastmod` 태그에 수정 날짜 입력**: `lastmod` 태그에는 마지막 수정 날짜를 입력하세요.

2. **사이트맵 파일 저장 위치**: `sitemap.xml` 파일은 블로그 Repository의 루트 디렉토리에 저장하세요. _config.yml 파일과 같은 위치

3. **Google Search Console에 사이트맵 제출**: 
   - **오른쪽 메뉴에서 Sitemaps 선택**: Google Search Console 대시보드에서 **Sitemaps** 메뉴로 이동하세요.
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_sitemap_side.png){: width="700" }
   - **사이트맵 URL 입력 후 제출**: 생성한 `sitemap.xml` 파일의 URL을 입력하고 제출하세요 (예: `https://zoren.github.io/sitemap.xml`). 
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_sitemap.png){: width="700" }

### Step 4. robots.txt 파일 설정

1. **robots.txt 파일 생성(필수)**: `robots.txt` 파일을 생성하여 검색 엔진이 크롤링할 수 있는 페이지와 크롤링하지 말아야 할 페이지를 설정하세요. 아래는 `robots.txt`의 예시입니다.

   ```
   User-agent: *
   Disallow: /admin/
   Sitemap: https://zoren.github.io/sitemap.xml
   ```

   - **`User-agent` 설정**: `User-agent`는 모든 검색 엔진을 대상으로 합니다.
   - **`Disallow` 설정**: 특정 경로를 검색 엔진이 크롤링하지 않도록 설정합니다.
   - **`Sitemap` 설정**: `Sitemap`에는 사이트맵의 URL을 입력하세요.

2. **robots.txt 파일 저장 위치**: `robots.txt` 파일 역시 블로그 Repository의 루트 디렉토리에 저장하세요. _config.yml 파일과 같은 위치

### Step 5. Google에서 블로그 검색 결과 확인

1. **블로그 이름 검색**: Google 검색창에 자신의 블로그 이름(예: `zoren github.io`)을 입력하고 검색 결과에 블로그가 나타나는지 확인하세요.
   - **Google 검색창 사용**: Google 검색창을 열고 블로그 이름을 정확히 입력한 후 검색하세요.

2. **검색 결과 확인**: 블로그 주소와 포스트가 정상적으로 검색 결과에 표시되는지 확인하세요.
   - ![이미지](https://doorimul-kjh.github.io/assets/img/posts/google_search_engine_search_result.png){: width="700" }