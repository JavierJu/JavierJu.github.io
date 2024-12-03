---
title: "JavaScript 조건부 실행: 조건 && 함수();를 사용하는 이유"
excerpt: "JavaScript에서 안전하게 함수를 호출하기 위해 조건 && 함수(); 패턴을 사용하는 이유를 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Conditional Execution, Coding Patterns]
permalink: /JavaScript/conditional-execution/
toc: true
toc_sticky: true
date: 2024-12-3
last_modified_at: 2024-12-3
---

JavaScript에서 `조건 && 함수();`라는 코드 패턴은 안전한 조건부 실행을 위해 자주 사용됩니다. 이 글에서는 이 패턴이 사용되는 이유와 작동 방식을 알아보고, 관련 예제를 통해 이해를 돕겠습니다.

## 조건 && 함수();가 필요한 이유

JavaScript에서 `&&` 연산자는 **조건부 실행**을 지원합니다. 이 패턴은 다음 상황에서 유용합니다:

1. 특정 조건이 **truthy**일 때만 함수를 호출하려고 할 때.
2. 조건이 **falsy**일 경우 아무 작업도 하지 않도록 하여 오류를 방지하려고 할 때.

### 예시 코드

아래 코드는 HTML의 배경색을 일정 시간 후에 변경하며, 조건부로 함수를 호출하는 예제를 보여줍니다.

``` js
const changeBackgroundColor = (newColor, delay, callback) => {
    setTimeout(() => {
        document.body.style.backgroundColor = newColor;
        callback && callback(); // callback이 있을 때만 실행
    }, delay);
};
```

### 함수 호출 시 발생할 수 있는 문제

``` plaintext
Uncaught TypeError: callback is not a function
```

`callback()`를 조건 없이 호출하면, `callback`이 정의되지 않거나 함수가 아닌 값일 때 오류가 발생합니다. 이로 인해 코드 실행이 중단되고, 예상치 못한 동작을 초래할 수 있습니다.

### 조건부 실행의 장점

`callback && callback();`를 사용하면 다음과 같은 이점이 있습니다:

- **조건이 truthy일 때만 호출**: `callback`이 함수일 때만 실행됩니다.
- **오류 방지**: `callback`이 `null`, `undefined`, 또는 다른 falsy 값이면 아무 작업도 하지 않고 넘어갑니다.

### 동작 원리

`&&` 연산자는 다음과 같이 작동합니다:

1. **첫 번째 피연산자 평가**: 첫 번째 조건이 falsy이면 바로 종료하며, 아무 작업도 하지 않습니다.
2. **두 번째 피연산자 평가**: 첫 번째 조건이 truthy라면 두 번째 조건을 평가합니다. 이 경우 함수가 실행됩니다.

이를 통해 **조건이 충족될 때만 함수 호출**이 가능합니다.

### 실전 예제: 조건부 실행 패턴

다음은 조건부 실행을 활용한 코드입니다:

``` js
const changeBackgroundColor = (newColor, delay, callback) => {
    setTimeout(() => {
        document.body.style.backgroundColor = newColor;
        // 안전하게 콜백 호출
        callback && callback();
    }, delay);
};

// 사용 예제
changeBackgroundColor('red', 1000, () => {
    changeBackgroundColor('blue', 1000, () => {
        changeBackgroundColor('green', 1000, null);
    });
});
```

위 코드에서:

1. 빨간색으로 배경색이 변경된 후 파란색, 녹색 순으로 변경됩니다.
2. 마지막 호출에서는 `callback`으로 `null`을 전달해도 오류 없이 안전하게 동작합니다.

---

## 결론

`조건 && 함수();`는 JavaScript에서 간결하고 안전하게 함수를 호출하는 유용한 패턴입니다. 조건부 실행을 통해 코드 안정성을 높이고, 예상치 못한 오류를 방지할 수 있습니다. 선택적 실행이 필요한 상황에서 이 패턴을 적극적으로 활용해 보세요.
