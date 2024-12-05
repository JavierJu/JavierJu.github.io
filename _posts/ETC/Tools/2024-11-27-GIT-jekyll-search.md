---
title: "Jekyll 기반 GitHub 블로그에 검색 기능 추가하기"
excerpt: "Jekyll로 만든 GitHub Pages 블로그에 검색 기능을 추가하는 다양한 방법과 각각의 장단점을 알아봅니다."
categories:
  - Tools
tags:
  - [Jekyll, 검색, 블로그, GitHub]
permalink: /Tools/jekyll-search/
toc: true
toc_sticky: true
date: 2024-11-27
last_modified_at: 2024-11-27
---


Jekyll로 만든 GitHub Pages 블로그는 정적 사이트라는 특성상 기본적으로 검색 기능을 제공하지 않습니다. 하지만 몇 가지 방법을 통해 블로그에 검색 기능을 추가할 수 있습니다. 이번 글에서는 **Lunr.js**, **Simple Jekyll Search**, **Google Custom Search Engine(CSE)**, **Algolia**를 사용한 방법과 각각의 장단점을 비교해보겠습니다.

---

## 1. Jekyll 블로그에 검색 기능 추가 방법

### **1-1. Lunr.js**
Lunr.js는 Jekyll에서 정적 검색을 구현하는 데 널리 사용되는 JavaScript 라이브러리입니다.

#### **설치 방법**
1. Gemfile에 `jekyll-lunr-js-search` 플러그인 추가:
   ```ruby
   gem 'jekyll-lunr-js-search'
   ```
2. `_config.yml` 파일에서 플러그인 활성화:
   ```yml
   plugins:
     - jekyll-lunr-js-search
   lunr_search:
     output_dir: assets/js
     fields: 
       title: 10
       content: 5
       tags: 20
   ```
3. `_layouts` 파일에 검색 입력 창 및 검색 결과 렌더링 스크립트 추가.

#### **장점**
- 오프라인에서도 작동 (완전 정적 검색).
- 커스터마이징이 용이하며, 제목, 태그 등 검색 우선순위를 설정 가능.

#### **단점**
- 데이터가 많아질수록 검색 속도가 느려질 수 있음.
- 초기 설정이 비교적 복잡.

---

### **1-2. Simple Jekyll Search**
Simple Jekyll Search는 설정이 간단하고 초보자에게 적합한 검색 기능 구현 라이브러리입니다.

#### **설치 방법**
1. `simple-jekyll-search.min.js` 파일을 다운로드하여 블로그에 추가.
2. 블로그 루트에 `search.json` 파일 생성:
   ```json
   [
     {
       "title": "포스트 제목",
       "url": "/post-url",
       "date": "yyyy-mm-dd",
       "tags": "태그1, 태그2"
     }
   ]
   ```
3. HTML 파일에 검색 입력창 및 검색 스크립트 추가:
   ```html
   <input type="text" id="search" placeholder="Search...">
   <script src="/path-to/simple-jekyll-search.min.js"></script>
   <script>
     SimpleJekyllSearch({
       searchInput: document.getElementById('search'),
       resultsContainer: document.getElementById('results'),
       json: '/search.json'
     });
   </script>
   ```

#### **장점**
- 구현이 매우 간단하고 초보자도 쉽게 설정 가능.
- 커스터마이징이 가능하며, JSON 형식으로 검색 데이터 관리.

#### **단점**
- 데이터가 많아질 경우 성능 저하.
- 정적 JSON 파일에만 의존하므로 실시간 업데이트 불가.

---

### **1-3. Google Custom Search Engine (CSE)**
Google CSE는 Google이 제공하는 검색 서비스를 활용하여 블로그에 검색 기능을 추가하는 방법입니다.

#### **설치 방법**
1. [Google CSE](https://cse.google.com/)에서 새로운 검색 엔진 생성.
2. 블로그 URL을 추가하여 검색 대상 등록.
3. CSE에서 제공하는 코드를 복사하여 블로그에 삽입:
   ```html
   <script async src="https://cse.google.com/cse.js?cx=YOUR_SEARCH_ENGINE_ID"></script>
   <div class="gcse-search"></div>
   ```

#### **장점**
- Google 검색 알고리즘 활용 (정확도 높음).
- 설정이 간단하며 실시간 데이터 반영 가능.
- 무료 사용 가능.

#### **단점**
- 검색 결과에 Google 광고가 포함될 수 있음.
- 완전한 UI 커스터마이징은 어려움.

---

### **1-4. Algolia**
Algolia는 고급 검색 기능을 제공하는 클라우드 기반 검색 엔진입니다.

#### **설치 방법**
1. Algolia 계정을 생성하고 애플리케이션 ID 및 API 키 발급.
2. 블로그 데이터를 Algolia에 인덱싱 (예: `jekyll-algolia` 플러그인 사용).
   ```ruby
   bundle exec jekyll algolia
   ```
3. JavaScript를 통해 검색 결과 표시:
   ```js
   <script src="https://cdn.jsdelivr.net/algoliasearch/3/algoliasearch.min.js"></script>
   <script>
     var client = algoliasearch('YOUR_APPLICATION_ID', 'YOUR_SEARCH_ONLY_API_KEY');
     var index = client.initIndex('YOUR_INDEX_NAME');
     // 검색 결과 렌더링 코드 추가
   </script>
   ```

#### **장점**
- 검색 속도가 매우 빠르고 대규모 데이터에 적합.
- 검색 결과에 정렬, 필터 등 고급 기능 제공.
- UI 커스터마이징 가능.

#### **단점**
- 무료 플랜에서 사용량 제한.
- 초기 설정이 복잡하고 Jekyll 외의 추가 작업 필요.

---

## 2. 각 방법의 비교

| **기능/방법**          | **Lunr.js**           | **Simple Jekyll Search** | **Google CSE**              | **Algolia**               |
|------------------------|-----------------------|--------------------------|-----------------------------|---------------------------|
| **설정 난이도**         | 보통                  | 쉬움                     | 쉬움                        | 어려움                    |
| **검색 속도**           | 느림 (큰 데이터에 불리함) | 느림                     | 빠름                        | 매우 빠름                 |
| **검색 정확도**         | 보통                  | 보통                     | 매우 정확                   | 매우 정확                 |
| **실시간 데이터 반영**   | 불가능                | 불가능                   | 가능                        | 가능                      |
| **무료 사용 가능 여부**  | 가능                  | 가능                     | 가능                        | 제한적 (무료 플랜 제한)    |
| **커스터마이징**        | 가능                  | 가능                     | 제한적                      | 매우 가능                 |

---

## 3. 추천 방법

1. **간단한 블로그**: 
   - `Simple Jekyll Search` 또는 `Google CSE` 추천.
   - 설정이 쉽고 소규모 블로그에 적합.

2. **고급 검색 기능 필요**: 
   - 데이터가 많거나 고급 검색 필터가 필요하면 `Algolia` 추천.

3. **오프라인 검색 필요**:
   - 오프라인에서도 동작해야 한다면 `Lunr.js` 사용.

---

검색 기능은 블로그의 사용성을 크게 향상시키는 중요한 요소입니다. 블로그의 특성과 목적에 맞는 방법을 선택해보세요!
