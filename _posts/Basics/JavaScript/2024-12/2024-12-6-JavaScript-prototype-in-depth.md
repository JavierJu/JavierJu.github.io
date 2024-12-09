---
title: "JavaScript에서 객체 프로토타입의 이해"
excerpt: "자바스크립트의 객체 프로토타입 개념과 이를 활용한 상속 및 객체 지향 프로그래밍 방법을 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, 프로토타입, 상속, 객체지향, 프로그래밍]
permalink: /JavaScript/prototype-in-depth/
toc: true
toc_sticky: true
date: 2024-12-6
last_modified_at: 2024-12-6
---

JavaScript의 객체 프로토타입은 객체와 상속의 핵심 개념으로, 자바스크립트가 객체 지향 프로그래밍을 지원하는 방식의 기본을 형성합니다. 아래는 객체 프로토타입에 대한 자세한 설명입니다.

## 1. 객체와 프로토타입

모든 자바스크립트 객체는 숨겨진 링크(혹은 내부 프로퍼티)인 `[[Prototype]]`를 가지고 있습니다. 이 링크는 다른 객체를 참조하며, 이를 통해 상속을 구현합니다. `[[Prototype]]`은 명시적으로 접근할 수 없지만, `Object.getPrototypeOf`나 `__proto__`를 사용하여 확인하거나 변경할 수 있습니다.

```js
const obj = {};
console.log(Object.getPrototypeOf(obj)); // [Object: null prototype] {}
console.log(obj.__proto__); // 동일한 결과
```

## 2. 프로토타입 체인

프로토타입 체인은 객체가 특정 프로퍼티나 메서드를 찾을 때 `[[Prototype]]`을 따라가는 메커니즘입니다.

- 객체에 원하는 속성이 없으면 프로토타입 체인을 따라 위로 올라가면서 속성을 찾습니다.
- 프로토타입 체인의 끝은 항상 `null`입니다. `null`은 더 이상 상속받을 프로토타입이 없음을 의미합니다.

```js
const obj = { a: 1 };
console.log(obj.toString()); 
// `obj`에는 `toString` 메서드가 없으므로, 프로토타입 체인을 따라 올라가서 Object.prototype의 toString을 호출합니다.
```

## 3. Object.prototype

`Object.prototype`은 모든 객체의 최상위 프로토타입입니다. 자바스크립트의 모든 객체는 암묵적으로 `Object.prototype`을 상속받습니다.

- `Object.prototype`에는 `toString`, `valueOf`, `hasOwnProperty` 등 자주 사용되는 메서드가 정의되어 있습니다.
- 필요에 따라 사용자 정의 객체에서 이 메서드들을 재정의할 수 있습니다.

```js
console.log(Object.prototype); // {constructor: ƒ, toString: ƒ, hasOwnProperty: ƒ, ...}
```

## 4. 프로토타입을 사용한 상속

자바스크립트에서 상속은 프로토타입을 통해 구현됩니다. 한 객체를 다른 객체의 프로토타입으로 설정하면 그 객체는 다른 객체의 프로퍼티와 메서드를 상속받을 수 있습니다.

### 예제 1: 수동으로 프로토타입 설정

```js
const parent = { greet: () => console.log('Hello!') };
const child = Object.create(parent);

child.greet(); // Hello! 
console.log(Object.getPrototypeOf(child) === parent); // true
```

### 예제 2: 함수 생성자와 prototype 속성

```js
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function() {
  console.log(`Hi, my name is ${this.name}`);
};

const alice = new Person('Alice');
alice.sayHello(); // Hi, my name is Alice
console.log(alice.__proto__ === Person.prototype); // true
```

## 5. 클래스 구문과 프로토타입

ES6에서 도입된 `class` 구문은 프로토타입 기반 상속을 더 읽기 쉽게 표현한 문법입니다. 내부적으로 `class`는 프로토타입을 사용합니다.

```js
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    console.log(`${this.name} makes a noise.`);
  }
}

class Dog extends Animal {
  speak() {
    console.log(`${this.name} barks.`);
  }
}

const dog = new Dog('Rex');
dog.speak(); // Rex barks.
console.log(Object.getPrototypeOf(dog) === Dog.prototype); // true
```

## 6. 프로토타입 변경

### Object.create로 새로운 프로토타입 설정

```js
const proto = { sayHi: () => console.log('Hi!') };
const obj = Object.create(proto);
obj.sayHi(); // Hi!
```

### __proto__와 Object.setPrototypeOf

`__proto__`는 객체의 프로토타입을 설정하거나 확인할 수 있는 비표준 속성이지만, 최신 환경에서는 대신 `Object.setPrototypeOf`와 `Object.getPrototypeOf`를 사용하는 것이 권장됩니다.

```js
const obj = {};
const proto = { greet: () => console.log('Hello!') };

Object.setPrototypeOf(obj, proto);
obj.greet(); // Hello!
```

## 7. 프로토타입의 장단점

### 장점

- **코드 재사용**: 여러 객체에서 동일한 메서드와 프로퍼티를 공유할 수 있습니다.
- **유연성**: 런타임에 프로토타입을 수정하거나 확장할 수 있습니다.

### 단점

- **속도 문제**: 프로토타입 체인이 길면 속성이 검색될 때 성능이 저하될 수 있습니다.
- **코드 이해도**: 너무 많은 프로토타입 체인은 디버깅을 어렵게 할 수 있습니다.

## 8. 주의사항

- `for...in` 루프는 프로토타입 체인에서 상속받은 열거 가능한 프로퍼티도 포함합니다. 따라서 `hasOwnProperty`로 확인하는 것이 중요합니다.
- 프로토타입 체인은 무한 루프를 생성하지 않도록 설정해야 합니다.

```js
const obj = {};
obj.__proto__ = obj; // 잘못된 설정으로 인해 문제가 발생할 수 있음
```

## 요약

JavaScript의 프로토타입은 객체 간 상속과 재사용을 가능하게 하는 강력한 메커니즘입니다. 기본 개념을 이해하고 적절히 사용하면 코드의 유연성과 재사용성을 크게 향상시킬 수 있습니다.
