---
title: "JavaScript 객체 리터럴 완벽 가이드"
excerpt: "JavaScript 객체 리터럴 생성, 외부 데이터 접근, 수정, 배열 및 객체 네스트 구성 방법을 다양한 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Objects, Programming, Web Development]
permalink: /JavaScript/object-literals-guide/
toc: true
toc_sticky: true
date: 2024-11-21
last_modified_at: 2024-11-21
---

JavaScript의 객체 리터럴은 직관적이고 간단하게 객체를 생성하는 방법입니다. 이 글에서는 객체 리터럴의 생성 방법부터 데이터 접근, 수정, 네스트 구성까지 다양한 예제와 함께 알아보겠습니다.

---

## 객체 리터럴 생성하기

객체 리터럴은 중괄호 `{}` 안에 키-값 쌍을 정의하여 생성합니다. 

### 기본 예제

```js
const person = {
  name: "John Doe",
  age: 30,
  isStudent: false
};

console.log(person);
```

위 코드에서 `person` 객체는 이름, 나이, 그리고 학생 여부를 나타내는 세 가지 속성을 포함하고 있습니다.

---

## 객체 데이터에 접근하기

객체의 데이터는 **점 표기법(dot notation)**과 **대괄호 표기법(bracket notation)**을 통해 접근할 수 있습니다.

### 예제: 데이터 접근

```js
const person = {
  name: "John Doe",
  age: 30,
  isStudent: false
};

// 점 표기법
console.log(person.name); // "John Doe"

// 대괄호 표기법
console.log(person["age"]); // 30
```

대괄호 표기법은 키가 동적으로 결정되거나, 키에 공백이나 특수 문자가 포함된 경우 유용합니다.

```js
const dynamicKey = "isStudent";
console.log(person[dynamicKey]); // false
```

---

## 객체 데이터 수정하기

객체의 속성은 점 표기법이나 대괄호 표기법으로 수정할 수 있습니다.

### 예제: 데이터 수정

```js
const person = {
  name: "John Doe",
  age: 30
};

// 새 속성 추가
person.isStudent = true;

// 기존 속성 수정
person.age = 31;

// 속성 삭제
delete person.name;

console.log(person);
// { age: 31, isStudent: true }
```

---

## 배열과 객체의 네스트 구성

객체는 배열 안에 포함되거나, 객체 안에 다른 객체를 포함할 수 있습니다.

### 예제: 배열 안에 객체

```js
const users = [
  { id: 1, name: "Alice" },
  { id: 2, name: "Bob" },
  { id: 3, name: "Charlie" }
];

console.log(users[1].name); // "Bob"
```

### 예제: 객체 안에 객체

```js
const company = {
  name: "TechCorp",
  employees: {
    manager: { name: "Sarah", age: 40 },
    engineer: { name: "Tom", age: 25 }
  }
};

console.log(company.employees.manager.name); // "Sarah"
```

---

## 객체의 반복 처리

객체의 키와 값을 순회하려면 `for...in` 문 또는 `Object` 메서드를 사용할 수 있습니다.

### 예제: 키-값 순회

```js
const person = {
  name: "John Doe",
  age: 30,
  isStudent: false
};

// for...in
for (let key in person) {
  console.log(`${key}: ${person[key]}`);
}

// Object.entries
Object.entries(person).forEach(([key, value]) => {
  console.log(`${key}: ${value}`);
});
```

---

JavaScript 객체 리터럴은 유연하고 강력한 데이터 구조를 제공합니다. 다양한 상황에서 객체를 효과적으로 활용해 코드를 더욱 간결하고 직관적으로 작성할 수 있습니다.
