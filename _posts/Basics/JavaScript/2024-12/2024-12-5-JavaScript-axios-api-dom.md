---
title: "Axios로 API 호출 및 DOM 조작 연습하기"
excerpt: "Axios를 활용한 API 호출 및 DOM 조작 방법을 알아보고, 헤더 설정과 에러 처리까지 다뤄봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Axios, API, DOM Manipulation]
permalink: /JavaScript/axios-api-dom/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

### Axios를 활용한 API 호출 연습

이 글에서는 Axios를 활용하여 API 호출을 수행하고, 반환된 데이터를 DOM에 표시하는 간단한 애플리케이션을 만들어 보겠습니다. API 호출 시 **헤더 설정**과 에러 처리 방법도 다룹니다.

---

#### 1. API 호출과 Accept 헤더 설정

API 요청 시, 특정 데이터 형식을 반환받기 위해 **헤더**를 설정해야 하는 경우가 많습니다. 예를 들어, `Accept` 헤더를 `application/json`으로 지정하지 않으면 HTML 형식의 응답을 받을 수도 있습니다. 이를 해결하려면 Axios의 두 번째 인자로 **설정 객체**를 전달해야 합니다.

```js
const getDadJoke = async () => {
  const config = {
    headers: { Accept: 'application/json' },
  };
  const res = await axios.get('https://icanhazdadjoke.com', config);
  console.log(res.data.joke); // 받은 개그 출력
};
getDadJoke();
```

---

#### 2. DOM 조작: 버튼 클릭 시 새로운 데이터 추가

API 호출로 얻은 데이터를 페이지에 동적으로 추가하려면 **DOM 조작**이 필요합니다. 아래는 개그를 `<ul>` 목록에 추가하는 예제입니다.

```html
<h1>Click to get new jokes!</h1>
<button>Click me!</button>
<ul id="jokes"></ul>
```

JavaScript 코드는 다음과 같습니다:

```js
const jokes = document.querySelector('#jokes');
const button = document.querySelector('button');

const addNewJoke = async () => {
  const jokeText = await getDadJoke();
  const newLI = document.createElement('li');
  newLI.append(jokeText);
  jokes.append(newLI);
};

button.addEventListener('click', addNewJoke);
```

---

#### 3. 에러 처리 추가하기

API 호출 중 네트워크 오류나 기타 문제가 발생할 경우를 대비해 **에러 처리**를 추가합니다. 예를 들어, 오류 발생 시 기본 메시지를 표시하도록 할 수 있습니다.

```js
const getDadJoke = async () => {
  try {
    const config = {
      headers: { Accept: 'application/json' },
    };
    const res = await axios.get('https://icanhazdadjoke.com', config);
    return res.data.joke;
  } catch (error) {
    console.error('Error fetching joke:', error);
    return "NO JOKES AVAILABLE! SORRY :(";
  }
};
```

---

#### 4. Axios 사용 시 주의 사항

- **속도 제한**: API 호출에는 일반적으로 속도 제한이 있습니다. 너무 자주 호출하면 IP가 차단될 수 있으니 주의하세요.
- **에러 처리**: 항상 네트워크나 API의 문제를 대비해 에러 처리 구문을 추가하세요.
- **헤더 설정**: 요청 시 API가 요구하는 형식에 맞게 헤더를 설정하세요.

---

#### 요약

이 글에서는 Axios를 활용해 API 데이터를 호출하고, DOM에 동적으로 추가하는 방법을 배웠습니다. API 요청 시 적절한 **헤더 설정**과 **에러 처리**가 중요하며, DOM 조작을 통해 사용자의 경험을 개선할 수 있습니다. 다음 글에서는 다른 API를 활용해 **쿼리 문자열**을 다루는 예제를 살펴보겠습니다.
