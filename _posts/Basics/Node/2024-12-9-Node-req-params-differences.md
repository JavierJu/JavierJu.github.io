---
title: "Express: req.params의 구조 분해 할당과 점 표기법의 차이"
excerpt: "Express에서 req.params 객체를 다루는 두 가지 접근 방식, 구조 분해 할당과 점 표기법의 차이를 비교하며 각각의 장단점을 살펴봅니다."
categories:
  - Node
tags:
  - [JavaScript, Express, Backend, Web Development]
permalink: /Node/req-params-differences/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

Express 애플리케이션에서 `req.params` 객체를 사용하는 방법에는 **구조 분해 할당**과 **점 표기법** 두 가지가 있습니다. 아래는 두 접근 방식의 코드와 차이점을 설명합니다.

---

## 코드 비교

### 1. 구조 분해 할당 방식
```js
app.get('/r/:subreddit', (req, res) => {
    const { subreddit } = req.params;
    res.send(`<h1>This is ${subreddit} subreddit!<h1>`);
});
```

- **특징**:
  - `req.params` 객체에서 특정 키(`subreddit`)를 추출하는 데 **구조 분해 할당** 문법을 사용합니다.
  - 코드를 짧고 간결하게 작성할 수 있습니다.
  - 여러 값을 동시에 추출해야 하는 경우에 특히 유용합니다.
    ```js
    const { subreddit, postId } = req.params;
    ```

---

### 2. 점 표기법 방식
```js
app.get('/r/:subreddit', (req, res) => {
    const subreddit = req.params.subreddit;
    res.send(`<h1>This is ${subreddit} subreddit!<h1>`);
});
```

- **특징**:
  - `req.params` 객체에 직접 접근하여 값을 추출합니다.
  - 익숙한 접근 방식으로 명확하게 동작합니다.
  - 간단한 코드에서는 문제 없이 사용할 수 있습니다.

---

## 차이점 및 선택 기준

### 공통점
- 두 방식 모두 동일하게 작동하며 결과에 차이가 없습니다.
- 위 코드 예제에서 두 코드의 실행 결과는 다음과 같습니다.

```bash
요청: GET /r/javascript 응답: <h1>This is javascript subreddit!</h1>
```

### 차이점

| 구조 분해 할당                       | 점 표기법                             |
|--------------------------------------|---------------------------------------|
| 코드가 간결하며 유지 보수가 용이합니다. | 기본적인 접근 방식으로 직관적입니다.  |
| 여러 값을 한 번에 추출하기 적합합니다.  | 단일 값 추출에 적합합니다.            |
| 최신 JavaScript 문법을 사용합니다.      | 전통적인 접근 방식으로 간단합니다.    |

---

## 결론
- **구조 분해 할당** 방식은 더 깔끔하고 현대적인 JavaScript 코드 스타일을 제공합니다. 특히 여러 개의 변수를 다루는 경우 유용합니다.
- **점 표기법** 방식은 익숙하고 간단한 작업에 적합합니다.

작업 환경이나 팀의 코드 스타일 가이드에 따라 적합한 방법을 선택하면 됩니다.
