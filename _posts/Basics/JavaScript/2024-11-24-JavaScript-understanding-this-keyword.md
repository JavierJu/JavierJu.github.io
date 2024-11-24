---
title: "JavaScript에서 'this' 키워드 완벽 이해하기: 다양한 상황과 예제"
excerpt: "'this' 키워드는 JavaScript에서 함수 호출 방식에 따라 달라지는 컨텍스트를 참조합니다. 다양한 예제를 통해 'this'의 동작 원리를 완벽히 이해해 보세요."
categories:
  - JavaScript
tags:
  - [JavaScript, Functions, Objects, Arrow Functions, Context]
permalink: /JavaScript/understanding-this-keyword/
toc: true
toc_sticky: true
date: 2024-11-24
last_modified_at: 2024-11-24
---

JavaScript의 `this` 키워드는 함수 호출 방식에 따라 달라지는 컨텍스트를 참조하기 때문에 처음 접할 때 혼란스러울 수 있습니다. 코드 작성 시점이 아닌 실행 시점에 결정된다는 점에서 많은 개발자들이 어려움을 겪습니다. 아래에서 다양한 상황별로 `this`가 어떻게 동작하는지 자세히 살펴보겠습니다.

---

## 1. 일반 함수에서의 `this`
일반 함수에서 `this`는 기본적으로 **전역 객체**를 가리킵니다.
- 브라우저 환경: `window`
- Node.js 환경: `global`

```js
function showThis() {
    console.log(this);
}

// 브라우저 환경에서 호출
showThis(); // window 객체 출력

// Node.js 환경에서 호출
// showThis(); // global 객체 출력
```
### `use strict`를 사용한 경우
`"use strict"`를 사용하면 함수 내부의 `this`는 `undefined`가 됩니다.

```js
"use strict";

function showThis() {
    console.log(this);
}

showThis(); // undefined
```

---

## 2. 객체의 메서드에서의 `this`
객체의 메서드에서 `this`는 **메서드를 호출한 객체**를 참조합니다.

```js
const obj = {
    name: "JavaScript",
    showName: function () {
        console.log(this.name);
    }
};

obj.showName(); // "JavaScript"
```

하지만 메서드를 변수에 할당하거나 다른 곳으로 전달하면 `this`는 **전역 객체**를 가리킬 수 있습니다.

```js
const obj = {
    name: "JavaScript",
    showName: function () {
        console.log(this.name);
    }
};

const newFunction = obj.showName;
newFunction(); // undefined (엄격 모드에서는 전역 객체 대신 undefined)
```

---

## 3. 생성자 함수에서의 `this`
생성자 함수에서 `this`는 **새로 생성된 객체**를 참조합니다.

```js
function Person(name) {
    this.name = name;
}

const me = new Person("Javier");
console.log(me.name); // "Javier"
```

---

## 4. Arrow 함수에서의 `this`
화살표 함수의 `this`는 **자신을 감싸는 스코프**에서의 `this`를 상속받습니다.

```js
const obj = {
    name: "JavaScript",
    showName: function () {
        const arrow = () => console.log(this.name);
        arrow();
    }
};

obj.showName(); // "JavaScript"
```

---

## 5. 이벤트 핸들러에서의 `this`
DOM 이벤트 핸들러에서 `this`는 기본적으로 **이벤트가 바인딩된 DOM 요소**를 참조합니다.

```js
const button = document.querySelector("button");

button.addEventListener("click", function () {
    console.log(this); // 클릭된 버튼 요소
});

// 화살표 함수 사용 시
button.addEventListener("click", () => {
    console.log(this); // window
});
```

---

## 6. `bind`, `call`, `apply`로 `this` 제어하기
`this`를 명시적으로 설정할 수 있는 메서드들입니다.

- **`call`**: 함수 호출 시 `this`와 인수를 직접 전달.
- **`apply`**: `this`와 인수를 **배열** 형태로 전달.
- **`bind`**: `this`를 고정한 새 함수를 반환.

```js
function showThis(arg1, arg2) {
    console.log(this.name, arg1, arg2);
}

const obj = { name: "JavaScript" };

showThis.call(obj, "arg1", "arg2"); // "JavaScript arg1 arg2"
showThis.apply(obj, ["arg1", "arg2"]); // "JavaScript arg1 arg2"

const boundFunc = showThis.bind(obj, "arg1");
boundFunc("arg2"); // "JavaScript arg1 arg2"
```

---

## 7. 클래스와 `this`
클래스 내부의 메서드에서 `this`는 생성된 인스턴스를 참조합니다.

```js
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        console.log(`${this.name} makes a noise.`);
    }
}

const dog = new Animal("Dog");
dog.speak(); // "Dog makes a noise."
```

---

## 8. `this`와 컨텍스트 변경 예제
아래 코드는 `this`의 다양한 동작을 보여줍니다.

```js
const obj = {
    name: "JavaScript",
    regularFunc: function () {
        console.log("regularFunc:", this.name);
    },
    arrowFunc: () => {
        console.log("arrowFunc:", this.name);
    }
};

obj.regularFunc(); // "regularFunc: JavaScript"
obj.arrowFunc(); // "arrowFunc: undefined"

const externalFunc = obj.regularFunc;
externalFunc(); // undefined
```

---

## 핵심 요약
1. 일반 함수: 전역 객체 (strict 모드에서는 undefined).
2. 메서드: 메서드를 호출한 객체.
3. 생성자: 새로 생성된 객체.
4. 화살표 함수: 자신을 감싸는 스코프의 `this`.
5. 명시적 호출: `call`, `apply`, `bind`로 설정 가능.

`this`를 다룰 때는 **호출 방법**과 **화살표 함수 사용 여부**를 항상 고려해야 합니다. 이 글이 `this`의 동작 방식을 이해하는 데 도움이 되었길 바랍니다!
