---
title: "JavaScript에서 함수 범위, 블록 범위, 렉시컬 범위 이해하기"
excerpt: "JavaScript의 변수 범위 개념인 함수 범위, 블록 범위, 렉시컬 범위에 대해 예제와 함께 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Scope, Lexical Scope, Function Scope, Block Scope, ES6]
permalink: /JavaScript/function-block-lexical-scope/
toc: true
toc_sticky: true
date: 2024-11-24
last_modified_at: 2024-11-24
---

JavaScript에서 변수가 어디에서 선언되느냐에 따라 유효 범위(scope)가 결정됩니다. 이 범위는 코드의 가독성과 오류 방지에 중요한 역할을 합니다. 이번 글에서는 함수 범위, 블록 범위, 그리고 렉시컬 범위를 예제와 함께 자세히 알아보겠습니다.

## 함수 범위(Function Scope)

함수 범위는 `var` 키워드로 선언된 변수가 **함수 내부에서만 유효**한 범위를 가집니다. 함수 외부에서는 해당 변수에 접근할 수 없습니다.

```js
function exampleFunctionScope() {
  var x = 10; // x는 함수 내부에서만 유효
  console.log("Inside function:", x);
}

exampleFunctionScope();
// console.log(x); // ReferenceError: x is not defined
```

### 주요 특징:
- `var` 키워드로 선언된 변수는 함수 스코프를 가집니다.
- 함수가 호출되지 않으면 함수 내부 변수는 생성되지 않습니다.

---

## 블록 범위(Block Scope)

ES6에서 도입된 `let`과 `const` 키워드는 **블록 범위**를 가집니다. 이는 `{}`로 묶인 블록 내부에서만 변수가 유효하다는 뜻입니다.

```js
{
  let a = 20; // 블록 내부에서만 유효
  const b = 30; // 블록 내부에서만 유효
  console.log("Inside block:", a, b);
}
// console.log(a); // ReferenceError: a is not defined
// console.log(b); // ReferenceError: b is not defined
```

### 주요 특징:
- `let`과 `const`로 선언된 변수는 블록 내부에서만 접근 가능합니다.
- 블록이 끝나면 해당 변수는 메모리에서 해제됩니다.

---

## 렉시컬 범위(Lexical Scope)

JavaScript는 **렉시컬 범위** 규칙을 따릅니다. 이는 함수가 **정의된 위치**를 기준으로 범위가 결정된다는 의미입니다. 함수 호출 위치는 영향을 미치지 않습니다.

```js
const outerVar = "I am outer";

function outerFunction() {
  const innerVar = "I am inner";
  
  function innerFunction() {
    console.log(outerVar); // 외부 스코프 변수에 접근 가능
    console.log(innerVar); // 동일 스코프나 상위 스코프 변수에 접근 가능
  }
  
  innerFunction();
}

outerFunction();
```

### 주요 특징:
- 함수 내부에서 상위 스코프의 변수에 접근할 수 있습니다.
- 호출 위치와 관계없이 함수가 선언된 위치를 기준으로 변수를 찾습니다.

---

## 함수 범위와 블록 범위 비교

함수 범위와 블록 범위를 비교하면 다음과 같은 차이점이 있습니다.

```js
function compareScopes() {
  if (true) {
    var functionScoped = "I am function scoped";
    let blockScoped = "I am block scoped";
  }
  
  console.log(functionScoped); // 정상 출력
  // console.log(blockScoped); // ReferenceError: blockScoped is not defined
}

compareScopes();
```

### 요약:
- `var`는 함수 범위를 가지며 함수 전체에서 접근할 수 있습니다.
- `let`과 `const`는 블록 범위를 가지며 해당 블록 내부에서만 유효합니다.

---

## 클로저(Closure)와 렉시컬 범위

렉시컬 범위 덕분에 JavaScript에서는 **클로저**가 가능합니다. 클로저는 함수가 외부 스코프의 변수에 접근할 수 있는 메커니즘입니다.

```js
function createCounter() {
  let count = 0; // 함수 스코프 변수
  
  return function () {
    count++; // 상위 함수의 변수에 접근 가능 (클로저)
    console.log("Count:", count);
  };
}

const counter = createCounter();
counter(); // Count: 1
counter(); // Count: 2
```

### 주요 특징:
- 내부 함수가 외부 함수의 변수에 계속 접근할 수 있습니다.
- 이를 통해 상태를 유지하거나 캡슐화된 데이터를 관리할 수 있습니다.

---

## 결론

JavaScript의 변수 범위를 이해하면 코드의 가독성과 디버깅이 쉬워집니다.  
다음은 주요 개념의 요약입니다:
1. **함수 범위**: `var`는 함수 내부에서만 유효합니다.
2. **블록 범위**: `let`과 `const`는 블록 내부에서만 유효합니다.
3. **렉시컬 범위**: 함수가 정의된 위치에 따라 범위가 결정됩니다.

위 개념을 기반으로 변수를 적절히 관리하고 코드를 작성해보세요! 😊
