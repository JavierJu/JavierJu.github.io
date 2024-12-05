---
title: "TVmaze API를 활용한 Axios 요청 연습 방법"
excerpt: "TVmaze API와 Axios를 사용해 데이터를 가져오는 방법을 간단한 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Axios, API, Web Development]
permalink: /JavaScript/tvmaze-api-axios/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

API를 활용한 데이터 처리 연습은 JavaScript 초보자가 실력을 키우기에 좋은 방법입니다. 이번 글에서는 **TVmaze API**와 **Axios**를 활용하여 데이터를 가져오는 방법을 소개합니다. 간단한 코드와 함께 시작해봅시다.

## 1. TVmaze API란?  
[TVmaze](https://www.tvmaze.com/api)는 TV 프로그램, 에피소드, 캐스트 등의 데이터를 제공하는 API입니다. 무료로 사용할 수 있고 초보자들이 연습하기에 적합합니다.

### 주요 Endpoint  
- **검색**: `https://api.tvmaze.com/search/shows?q={검색어}`
- **상세 정보**: `https://api.tvmaze.com/shows/{id}`

---

## 2. Axios 설치 및 기본 설정  

Axios는 JavaScript에서 HTTP 요청을 간편하게 처리할 수 있는 라이브러리입니다. 다음과 같은 명령어로 설치할 수 있습니다.

```bash
npm install axios
```

브라우저 환경에서는 CDN을 사용할 수도 있습니다.

```html
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
```

---

## 3. 기본 사용법: TV 프로그램 검색  

아래는 `search/shows` 엔드포인트를 사용해 TV 프로그램을 검색하는 예제입니다.

### 예제 코드  
```js
// Axios를 사용하여 TVmaze API에서 데이터를 가져오는 함수
async function searchShows(query) {
    const API_URL = `https://api.tvmaze.com/search/shows?q=${query}`;
    try {
        const response = await axios.get(API_URL);
        const shows = response.data; // API의 응답 데이터
        console.log(shows);
        return shows;
    } catch (error) {
        console.error("데이터를 가져오는 중 오류가 발생했습니다.", error);
    }
}

// 함수 호출
searchShows("Friends");
```

위 코드는 `Friends`라는 검색어로 TV 프로그램을 검색하고 결과를 콘솔에 출력합니다.

---

## 4. 결과 데이터를 HTML로 렌더링하기  

가져온 데이터를 사용자 인터페이스(UI)에 표시하려면 DOM 조작을 활용해야 합니다. 다음은 간단한 렌더링 예제입니다.

### 예제 코드  
```js
// 검색 결과를 HTML로 렌더링
function renderShows(shows) {
    const resultContainer = document.getElementById("result");
    resultContainer.innerHTML = ""; // 기존 내용을 초기화
    shows.forEach(({ show }) => {
        const showElement = document.createElement("div");
        showElement.className = "show-item";
        showElement.innerHTML = `
            <h2>${show.name}</h2>
            <p>${show.summary || "설명이 없습니다."}</p>
            <img src="${show.image?.medium || 'https://via.placeholder.com/210'}" alt="${show.name}">
        `;
        resultContainer.appendChild(showElement);
    });
}

// 이벤트 핸들러
document.getElementById("searchForm").addEventListener("submit", async (event) => {
    event.preventDefault();
    const query = document.getElementById("searchInput").value;
    const shows = await searchShows(query);
    renderShows(shows);
});
```

---

## 5. HTML 구조  

이 코드는 다음과 같은 HTML 구조와 함께 사용됩니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>TVmaze 검색</title>
    <style>
        .show-item { margin: 20px; }
        img { width: 210px; height: auto; }
    </style>
</head>
<body>
    <form id="searchForm">
        <input type="text" id="searchInput" placeholder="검색어를 입력하세요" required>
        <button type="submit">검색</button>
    </form>
    <div id="result"></div>
    <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
    <script src="script.js"></script>
</body>
</html>
```

---

## 6. 마무리  

이 연습을 통해 JavaScript에서 API를 호출하고 데이터를 DOM에 렌더링하는 기본 과정을 익힐 수 있습니다. 더 나아가, 검색 필터 추가, 페이지네이션, 상세 보기 기능 등을 구현해보세요.

---
