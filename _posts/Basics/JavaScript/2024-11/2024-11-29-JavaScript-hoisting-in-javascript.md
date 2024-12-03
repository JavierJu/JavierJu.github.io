---
title: "자바스크립트에서 호이스팅(Hoisting) 이해하기"
excerpt: "자바스크립트에서 호이스팅 개념과 변수 및 함수 호이스팅의 차이를 이해하고, 이를 통해 발생할 수 있는 오류를 피하는 방법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Hoisting, 변수, 함수, 웹 개발]
permalink: /JavaScript/hoisting-in-javascript/
toc: true
toc_sticky: true
date: 2024-11-29
last_modified_at: 2024-11-29
---

자바스크립트에서 **호이스팅(Hoisting)**은 코드 실행 전에 변수와 함수 선언이 "끌어올려진다"는 개념입니다. 즉, 자바스크립트 엔진이 코드 실행을 시작하기 전에 **변수 선언과 함수 선언을 최상단으로 이동시킨다**는 원리입니다.

하지만 중요한 점은 **초기화**는 호이스팅되지 않으므로, 변수나 함수가 선언된 이후에만 그 값을 사용할 수 있다는 것입니다. 이로 인해 코드 실행 시 예기치 않은 동작을 할 수 있습니다.

## 1. 변수 호이스팅

변수 선언은 **var**, **let**, **const**와 같이 여러 가지 방법으로 할 수 있지만, 각각의 호이스팅 동작이 다릅니다.

### `var`로 선언한 변수

`var`로 선언한 변수는 선언 자체가 호이스팅되어 최상단으로 끌어올려집니다. 하지만 변수의 **초기화**는 호이스팅되지 않기 때문에, 선언 전에 변수에 접근하면 `undefined`가 반환됩니다.

``` js
console.log(myVar);  // undefined
var myVar = 10;
console.log(myVar);  // 10
```

이 예시에서 `myVar`는 최상단으로 끌어올려지지만, 값 `10`은 그 선언문 아래에서 할당되므로 처음 출력 시 `undefined`가 나옵니다.

### `let`과 `const`로 선언한 변수

`let`과 `const`는 변수의 호이스팅은 일어나지만, **"일시적 사각지대(Temporal Dead Zone, TDZ)"**라는 개념이 존재합니다. 이는 변수 선언 전에는 참조할 수 없다는 의미입니다. 따라서 선언 전에 값을 읽으려 하면 에러가 발생합니다.

``` js
console.log(myLet);  // ReferenceError: Cannot access 'myLet' before initialization
let myLet = 20;
```

`let`과 `const`는 선언된 이후에만 접근이 가능하고, 초기화 전에 접근하려고 하면 에러가 발생합니다.

## 2. 함수 호이스팅

함수 선언도 호이스팅됩니다. 함수 선언은 **전체**가 호이스팅되므로, 함수 선언 전에 호출해도 정상적으로 동작합니다.

``` js
myFunction();  // "Hello!"

function myFunction() {
  console.log("Hello!");
}
```

이 예시에서 `myFunction`은 선언 전에 호출되었지만, 정상적으로 작동합니다.

## 3. 함수 표현식의 호이스팅

함수 표현식(함수를 변수에 할당하는 방식)은 호이스팅되지 않습니다. 변수 선언은 호이스팅되지만, 함수의 실제 값은 할당된 이후에만 유효합니다.

``` js
myFunc();  // TypeError: myFunc is not a function
var myFunc = function() {
  console.log("Hello!");
};
```

여기서는 `myFunc`가 변수로 선언된 후 함수가 할당되기 때문에, 함수가 할당되기 전에는 `myFunc`가 `undefined`이고, 이를 함수처럼 호출하려 하면 에러가 발생합니다.

## 결론

호이스팅은 코드가 실행되기 전에 자바스크립트 엔진이 변수와 함수 선언을 최상단으로 이동시키는 현상입니다. 그러나 변수 초기화와 함수 표현식은 호이스팅되지 않으므로, 선언 전에 이를 사용하면 오류가 발생하거나 예기치 않은 동작이 일어날 수 있습니다. `var`와 `let`, `const`의 호이스팅 동작도 다르므로, 이를 명확히 이해하는 것이 중요합니다.
