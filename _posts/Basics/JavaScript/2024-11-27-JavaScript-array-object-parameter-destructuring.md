---
title: "JavaScript에서 배열 분해, 객체 분해, 매개변수 분해의 이해와 활용"
excerpt: "JavaScript의 구조 분해 할당을 활용하여 배열과 객체의 값을 효율적으로 추출하고, 함수 매개변수에서의 응용까지 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, ES6, 구조 분해 할당, 배열, 객체, 함수]
permalink: /JavaScript/array-object-parameter-destructuring/
toc: true
toc_sticky: true
date: 2024-11-27
last_modified_at: 2024-11-27
---

JavaScript에서 구조 분해 할당(Destructuring Assignment)은 코드를 더 간결하고 가독성 좋게 작성할 수 있게 해주는 강력한 기능입니다. 배열, 객체, 함수 매개변수에서 각각 어떻게 활용할 수 있는지 알아보겠습니다.

---

## 1. 배열 분해 (Array Destructuring)

배열의 요소를 변수로 추출하는 간단한 방법입니다.

### 기본 사용법
```js
const arr = [1, 2, 3];
const [a, b, c] = arr;

console.log(a); // 1
console.log(b); // 2
console.log(c); // 3
```

### 일부 요소만 추출
```js
const arr = [1, 2, 3];
const [, second, third] = arr; // 첫 번째 요소는 건너뜀

console.log(second); // 2
console.log(third); // 3
```

### 기본값 설정
```js
const arr = [1];
const [a, b = 2] = arr;

console.log(a); // 1
console.log(b); // 2
```

### 나머지 요소 추출 (Rest Syntax)
```js
const arr = [1, 2, 3, 4];
const [first, ...rest] = arr;

console.log(first); // 1
console.log(rest);  // [2, 3, 4]
```

---

## 2. 객체 분해 (Object Destructuring)

객체의 속성을 변수로 추출하여 더 쉽게 접근할 수 있습니다.

### 기본 사용법
```js
const obj = { x: 10, y: 20 };
const { x, y } = obj;

console.log(x); // 10
console.log(y); // 20
```

### 다른 변수 이름 사용
```js
const obj = { x: 10, y: 20 };
const { x: newX, y: newY } = obj;

console.log(newX); // 10
console.log(newY); // 20
```

### 기본값 설정
```js
const obj = { x: 10 };
const { x, y = 20 } = obj;

console.log(x); // 10
console.log(y); // 20
```

### 중첩 객체 분해
```js
const obj = { a: 1, b: { c: 2, d: 3 } };
const { b: { c, d } } = obj;

console.log(c); // 2
console.log(d); // 3
```

### 나머지 속성 추출 (Rest Syntax)
```js
const obj = { a: 1, b: 2, c: 3 };
const { a, ...rest } = obj;

console.log(a); // 1
console.log(rest); // { b: 2, c: 3 }
```

---

## 3. 매개변수 분해 (Parameter Destructuring)

함수의 매개변수로 전달된 객체나 배열의 값을 바로 추출할 수 있습니다.

### 배열 매개변수 분해
```js
function sum([a, b]) {
    return a + b;
}

console.log(sum([1, 2])); // 3
```

### 객체 매개변수 분해
```js
function greet({ name, age }) {
    return `Hello, my name is ${name} and I'm ${age} years old.`;
}

console.log(greet({ name: 'Alice', age: 25 })); 
// "Hello, my name is Alice and I'm 25 years old."
```

### 기본값 설정
```js
function greet({ name = 'Guest', age = 0 } = {}) {
    return `Hello, my name is ${name} and I'm ${age} years old.`;
}

console.log(greet({ name: 'Alice' })); // "Hello, my name is Alice and I'm 0 years old."
console.log(greet()); // "Hello, my name is Guest and I'm 0 years old."
```

---

## 실전 활용

### API 응답 데이터 처리
```js
const response = {
    data: { user: { id: 1, name: 'John' } },
    status: 200
};

const { data: { user: { name } }, status } = response;

console.log(name);  // "John"
console.log(status); // 200
```

### React에서 Props 분해
```js
function Component({ title, content }) {
    return (
        <div>
            <h1>{title}</h1>
            <p>{content}</p>
        </div>
    );
}
```

---

구조 분해 할당은 코드의 효율성과 가독성을 높이는 데 유용한 도구입니다. 프로젝트에 적절히 적용해 더 깔끔한 코드를 작성해 보세요!
