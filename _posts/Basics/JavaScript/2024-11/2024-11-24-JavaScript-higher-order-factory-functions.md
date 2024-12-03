---
title: "JavaScript 고차 함수와 팩토리 함수의 이해"
excerpt: "JavaScript 고차 함수와 팩토리 함수의 개념과 활용 방법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Functions, Higher-Order Functions, Factory Functions, Closure]
permalink: /JavaScript/higher-order-factory-functions/
toc: true
toc_sticky: true
date: 2024-11-24
last_modified_at: 2024-11-24
---

## 고차 함수란?

**고차 함수(Higher-Order Function)**는 다음 두 가지 조건 중 하나 이상을 만족하는 함수입니다:
- 함수를 **인수로 받는 함수**
- 함수를 **반환하는 함수**

JavaScript의 고차 함수는 **코드 재사용성**과 **추상화**를 높이는 데 중요한 역할을 하며, 함수형 프로그래밍의 핵심 개념 중 하나입니다.

---

### 고차 함수의 특징

1. **함수를 인수로 받을 수 있다.**
   - 특정 동작을 추상화하여 재사용할 수 있습니다.
2. **함수를 반환할 수 있다.**
   - 실행 결과로 동적으로 동작하는 새로운 함수를 반환합니다.
3. **추상화**와 **재사용성**을 극대화합니다.
   - 반복적인 작업을 간단히 처리할 수 있습니다.

---

### 고차 함수 예제

#### 1. 함수를 인수로 받는 경우

```js
function greet(name) {
    return `Hello, ${name}!`;
}

function processUserInput(callback) {
    const name = "Alice"; // 사용자로부터 입력받았다고 가정
    console.log(callback(name));
}

processUserInput(greet);
// 출력: Hello, Alice!
```

`processUserInput`는 `greet` 함수를 인수로 받아 실행하는 고차 함수입니다. JavaScript의 배열 메서드인 `map`, `filter`, `reduce`도 고차 함수의 대표적인 예입니다.

---

#### 2. 함수를 반환하는 경우

```js
function createMultiplier(multiplier) {
    return function (value) {
        return value * multiplier;
    };
}

const double = createMultiplier(2); // multiplier가 2인 함수 생성
const triple = createMultiplier(3); // multiplier가 3인 함수 생성

console.log(double(5)); // 출력: 10
console.log(triple(5)); // 출력: 15
```

`createMultiplier` 함수는 실행 결과로 새로운 함수를 반환하며, 이 함수는 클로저(Closure)로 동작합니다.

---

## 팩토리 함수란?

**팩토리 함수(Factory Function)**는 고차 함수의 한 종류로, **객체나 함수를 생성하여 반환하는 함수**입니다. 이를 통해 **유연하고 재사용 가능한 코드를 작성**할 수 있습니다.

---

### 팩토리 함수의 예제

#### 1. 객체 생성 팩토리 함수

```js
function createUser(name, role) {
    return {
        name,
        role,
        describe() {
            return `${this.name}는 ${this.role}입니다.`;
        },
    };
}

const user1 = createUser("Alice", "관리자");
const user2 = createUser("Bob", "사용자");

console.log(user1.describe()); // 출력: Alice는 관리자입니다.
console.log(user2.describe()); // 출력: Bob은 사용자입니다.
```

`createUser`는 객체를 반환하는 팩토리 함수로, 특정한 구조의 객체를 반복적으로 생성할 때 유용합니다.

---

#### 2. 상태를 기억하는 함수 생성

```js
function createCounter() {
    let count = 0; // 내부 상태 (클로저)

    return function () {
        count++;
        return count;
    };
}

const counter1 = createCounter();
const counter2 = createCounter();

console.log(counter1()); // 출력: 1
console.log(counter1()); // 출력: 2
console.log(counter2()); // 출력: 1
```

`createCounter`는 상태를 기억하는 클로저로 작동하며, 반환된 함수는 독립적으로 상태를 관리합니다.

---

## 고차 함수와 팩토리 함수의 차이점

- **고차 함수**는 일반적으로 함수형 프로그래밍에서 사용하는 패턴으로, 함수 간의 동작을 추상화합니다.
- **팩토리 함수**는 특정 동작을 캡슐화하여 객체나 상태를 가진 함수를 반환하는 고차 함수의 한 유형입니다.

---

## 고차 함수와 팩토리 함수의 장점

1. **코드 재사용성**: 반복되는 로직을 추상화하여 간결하고 유지보수하기 쉬운 코드를 작성할 수 있습니다.
2. **유연성**: 동적으로 함수를 생성하거나 특정 동작을 캡슐화할 수 있습니다.
3. **클로저와 결합**: 반환된 함수가 원래 함수의 상태를 기억할 수 있습니다.

---

고차 함수와 팩토리 함수는 JavaScript의 강력한 기능 중 하나로, 더 유연하고 유지보수 가능한 코드를 작성하는 데 큰 도움을 줍니다. 이를 이해하고 활용한다면 JavaScript 개발자로서 한 단계 더 성장할 수 있을 것입니다!
