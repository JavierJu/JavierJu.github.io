---
title: "Jekyll 블로그에서 다국어 지원 구현 방법"
excerpt: "Jekyll 블로그에서 방문자가 언어를 선택하면 다국어 콘텐츠를 표시할 수 있는 방법을 알아봅니다. 플러그인 사용, JavaScript 번역, API 활용 등 다양한 접근 방식을 소개합니다."
categories:
  - Jekyll
  - Multilingual
  - Git Blog
  - Git
  - Web Development
  
tags:
  - [Jekyll, 다국어 지원, 번역, 블로그, 웹 개발]
permalink: /git/jekyll-multilingual-support/
toc: true
toc_sticky: true
date: 2024-12-30
last_modified_at: 2024-12-30
---

## Jekyll 블로그에서 다국어 지원 구현 방법

GitHub 블로그(Jekyll)에서 방문자가 언어를 선택하면 자동 번역된 콘텐츠를 제공하는 기능을 구현하는 방법은 여러 가지가 있습니다. 이 글에서는 다음과 같은 접근 방식을 다룹니다:

1. [Jekyll 플러그인을 활용한 다국어 지원](#jekyll-플러그인을-활용한-다국어-지원)
2. [JavaScript와 JSON 파일을 이용한 번역](#javascript와-json-파일을-이용한-번역)
3. [Headless CMS와 통합](#headless-cms와-통합)
4. [AI 번역 API를 활용한 동적 번역](#ai-번역-api를-활용한-동적-번역)

---

## Jekyll 플러그인을 활용한 다국어 지원

Jekyll에서 다국어를 지원하려면 **`jekyll-multiple-languages-plugin`**과 같은 플러그인을 사용할 수 있습니다. 이 플러그인은 언어별로 콘텐츠를 작성하고 URL을 다르게 설정할 수 있도록 도와줍니다.

### 설정 방법

1. **Gemfile에 플러그인 추가**
   ```ruby
   gem "jekyll-multiple-languages-plugin"
   ```

2. **`_config.yml` 파일 설정**
   ```yaml
   languages:
     - en
     - ko
     - ja
   default_lang: ko
   ```

3. **언어별 게시물 생성**
   - `_posts/ko/` 폴더에 한국어 게시물 저장
   - `_posts/ja/` 폴더에 일본어 게시물 저장
   - `_posts/en/` 폴더에 영어 게시물 저장

4. **언어 전환 메뉴 추가**
   ```html
   <a href="/ja/">日本語</a> | <a href="/ko/">한국어</a> | <a href="/en/">English</a>
   ```

이 방법은 직접 번역된 콘텐츠를 제공해야 하지만, 정확한 번역을 보장할 수 있습니다.

---

## JavaScript와 JSON 파일을 이용한 번역

JavaScript와 JSON 파일을 이용하면 번역 데이터를 관리하고 자동으로 번역된 콘텐츠를 표시할 수 있습니다.

### 구현 방법

1. **JSON 파일 생성**
   - `locales/ko.json` (한국어)
   - `locales/ja.json` (일본어)
   ```json
   {
     "title": "안녕하세요!",
     "content": "이것은 테스트입니다."
   }
   ```

2. **JavaScript 코드 작성**
   ```html
   <script>
     async function changeLanguage(lang) {
       const response = await fetch(`/locales/${lang}.json`);
       const translations = await response.json();
       document.querySelectorAll("[data-translate]").forEach(el => {
         const key = el.getAttribute("data-translate");
         el.textContent = translations[key];
       });
     }

     document.addEventListener("DOMContentLoaded", () => {
       const userLang = navigator.language.slice(0, 2);
       changeLanguage(userLang === 'ko' ? 'ko' : 'ja');
     });
   </script>
   ```

3. **HTML 요소에 번역 키 추가**
   ```html
   <h1 data-translate="title"></h1>
   <p data-translate="content"></p>
   <button onclick="changeLanguage('ko')">한국어</button>
   <button onclick="changeLanguage('ja')">日本語</button>
   ```

---

## Headless CMS와 통합

Netlify CMS, Contentful 등과 같은 Headless CMS를 활용하면 다국어 콘텐츠 관리를 쉽게 할 수 있습니다. Headless CMS에서 각 언어별 콘텐츠를 생성하고 Jekyll과 연동하면, 블로그 포스트를 효율적으로 관리할 수 있습니다.

---

## AI 번역 API를 활용한 동적 번역

Google Cloud Translation API나 DeepL API를 사용하여 실시간으로 콘텐츠를 번역할 수도 있습니다.

### 구현 방법

1. **번역 API 호출**
   ```javascript
   async function translateContent(text, targetLang) {
     const response = await fetch(`https://api.example.com/translate?text=${encodeURIComponent(text)}&target=${targetLang}`);
     const data = await response.json();
     return data.translatedText;
   }

   async function applyTranslation(targetLang) {
     document.querySelectorAll("[data-translate]").forEach(async el => {
       const originalText = el.textContent;
       const translatedText = await translateContent(originalText, targetLang);
       el.textContent = translatedText;
     });
   }
   ```

2. **번역 전환 버튼 추가**
   ```html
   <button onclick="applyTranslation('ko')">한국어</button>
   <button onclick="applyTranslation('ja')">日本語</button>
   ```

---

## 최종 선택

- **정확성**이 중요하다면 Jekyll 플러그인을 사용해 직접 번역된 콘텐츠를 제공하세요.
- **자동 번역**이 필요하다면 JavaScript와 번역 API를 활용하는 방식을 고려하세요.

필요한 구현 방식이나 추가적인 예제가 있다면 언제든 댓글로 질문해주세요! 😊
