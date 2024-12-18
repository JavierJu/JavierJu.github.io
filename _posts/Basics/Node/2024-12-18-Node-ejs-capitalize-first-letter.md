---
title: "EJS에서 소문자 문자열의 첫 글자 대문자로 표시하기"
excerpt: "EJS 템플릿 엔진에서 문자열의 첫 글자를 대문자로 변환하는 방법을 알아봅니다. 간단한 JavaScript 코드를 활용해 텍스트 형식을 조정하세요."
categories:
  - Node
tags:
  - [EJS, JavaScript, 템플릿, 문자열 처리]
permalink: /Node/ejs-capitalize-first-letter/
toc: true
toc_sticky: true
date: 2024-12-18
last_modified_at: 2024-12-18
---

EJS 템플릿 엔진을 사용하다 보면 문자열 데이터를 표시할 때 특정 형식으로 변환해야 하는 경우가 있습니다. 예를 들어, 문자열이 모두 소문자로 되어 있을 때 첫 글자만 대문자로 표시하고 싶다면 아래 방법을 사용할 수 있습니다.

## 문제 상황
아래와 같이 `category` 변수의 값이 소문자 문자열로 주어졌다고 가정합니다:

```ejs
<h1>
    <%= category %> Products
</h1>
```

이 경우 `category`의 값이 `"electronics"`라면 "electronics Products"로 출력됩니다. 이를 "Electronics Products"로 변환하려면 첫 글자를 대문자로 바꾸는 로직이 필요합니다.

## 해결 방법
JavaScript 문자열 메서드를 활용하여 `category`의 첫 글자를 대문자로 변환할 수 있습니다. 다음과 같이 코드를 수정해 보세요:

```ejs
<h1>
    <%= category.charAt(0).toUpperCase() + category.slice(1) %> Products
</h1>
```

### 코드 설명
1. **`category.charAt(0)`**:
   - 문자열 `category`의 첫 번째 문자를 가져옵니다.

2. **`.toUpperCase()`**:
   - 첫 번째 문자를 대문자로 변환합니다.

3. **`category.slice(1)`**:
   - 문자열의 첫 번째 문자를 제외한 나머지 부분을 가져옵니다.

4. **`+` 연산자**:
   - 대문자로 변환한 첫 글자와 나머지 문자열을 합쳐 새로운 문자열을 만듭니다.

### 결과 예시
`category` 값이 `"electronics"`라면 위 코드는 다음과 같이 출력됩니다:

```html
<h1>
    Electronics Products
</h1>
```

## 요약
EJS에서 소문자로 구성된 문자열의 첫 글자를 대문자로 변환하려면 JavaScript의 문자열 메서드인 `charAt`, `toUpperCase`, 및 `slice`를 조합하여 간단하게 처리할 수 있습니다. 이 방법을 활용하면 템플릿 데이터를 보다 유연하게 표시할 수 있습니다.

