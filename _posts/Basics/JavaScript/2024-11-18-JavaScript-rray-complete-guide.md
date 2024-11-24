---
title: "JavaScript 배열 완벽 가이드: 생성, 메서드, 활용법 총정리"
excerpt: "JavaScript 배열의 생성부터 다양한 메서드 활용법까지, 배열을 효율적으로 사용하는 방법을 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Array, Programming, Web Development]
permalink: /JavaScript/array-complete-guide/
toc: true
toc_sticky: true
date: 2024-11-18
last_modified_at: 2024-11-18
---

JavaScript에서 배열은 데이터를 순서대로 저장하고 조작할 수 있는 강력한 도구입니다. 이 포스트에서는 배열의 기본 개념, 주요 메서드, 그리고 활용법을 알아봅니다.

## 배열 생성 방법
JavaScript에서 배열은 여러 방식으로 생성할 수 있습니다.

```js
const fruits = ["apple", "banana", "cherry"]; // 리터럴 방식
const numbers = new Array(1, 2, 3, 4, 5);    // Array 생성자
const emptyArray = [];                       // 빈 배열
```

## 배열의 특징
- **인덱스 기반**: 배열의 인덱스는 0부터 시작합니다.
- **다양한 데이터 타입 저장 가능**: 문자열, 숫자, 객체 등 모든 데이터 타입을 혼합해서 저장할 수 있습니다.
- **동적 크기 조절**: 요소 추가/삭제에 따라 배열의 길이가 자동으로 조정됩니다.

## 배열 주요 메서드

### 배열 크기 확인
배열의 길이는 `length` 속성으로 확인할 수 있습니다.

```js
const fruits = ["apple", "banana", "cherry"];
console.log(fruits.length); // 3
```

### 요소 추가 및 삭제
- **끝에 요소 추가**: `push`
- **끝에서 요소 제거**: `pop`
- **앞에 요소 추가**: `unshift`
- **앞에서 요소 제거**: `shift`

```js
const fruits = ["apple", "banana"];
fruits.push("cherry");  // ["apple", "banana", "cherry"]
fruits.pop();           // ["apple", "banana"]
fruits.unshift("grape"); // ["grape", "apple", "banana"]
fruits.shift();          // ["apple", "banana"]
```

### 배열 순회
배열 요소를 순회하는 다양한 방법이 있습니다.

```js
const fruits = ["apple", "banana", "cherry"];
// 기본 for문
for (let i = 0; i < fruits.length; i++) {
    console.log(fruits[i]);
}

// for...of
for (const fruit of fruits) {
    console.log(fruit);
}

// forEach
fruits.forEach((fruit, index) => {
    console.log(`${index}: ${fruit}`);
});
```

### 요소 찾기
- 특정 요소의 **인덱스 찾기**: `indexOf`
- 조건에 맞는 **첫 번째 요소 찾기**: `find`
- **포함 여부 확인**: `includes`

```js
const fruits = ["apple", "banana", "cherry"];
console.log(fruits.indexOf("banana")); // 1
console.log(fruits.find(fruit => fruit.startsWith("b"))); // "banana"
console.log(fruits.includes("apple")); // true
```

### 배열 변형
- **정렬**: `sort`
- **역순 정렬**: `reverse`
- **배열 병합**: `concat`
- **배열 슬라이스**: `slice`
- **특정 요소 대체/추가/삭제**: `splice`

```js
const numbers = [4, 2, 5, 1, 3];
numbers.sort();       // [1, 2, 3, 4, 5]
numbers.reverse();    // [5, 4, 3, 2, 1]
const newArray = numbers.concat([6, 7]); // [5, 4, 3, 2, 1, 6, 7]
const sliced = numbers.slice(1, 3);      // [4, 3]
numbers.splice(2, 1, 10);                // [5, 4, 10, 2, 1]
```

### 배열 생성 및 변환
- **배열 생성**: `map`
- **조건에 맞는 요소 필터링**: `filter`
- **모든 요소 합산**: `reduce`

```js
const numbers = [1, 2, 3, 4, 5];
const squares = numbers.map(num => num ** 2);        // [1, 4, 9, 16, 25]
const evenNumbers = numbers.filter(num => num % 2 === 0); // [2, 4]
const sum = numbers.reduce((acc, num) => acc + num, 0); // 15
```

## 다차원 배열
JavaScript에서는 배열 안에 배열을 저장할 수 있습니다.

```js
const matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
];
console.log(matrix[1][2]); // 6
```

## 배열 활용 팁
1. 배열 크기를 조정하려면 `length` 속성을 수정하세요.
   ```js
   const arr = [1, 2, 3, 4, 5];
   arr.length = 3;
   console.log(arr); // [1, 2, 3]
   ```

2. 유사 배열 객체를 배열로 변환할 때는 `Array.from`을 사용하세요.
   ```js
   const obj = {0: "a", 1: "b", 2: "c", length: 3};
   const arr = Array.from(obj);
   console.log(arr); // ["a", "b", "c"]
   ```

---

JavaScript 배열은 다양한 작업을 간단하게 처리할 수 있는 유용한 도구입니다. 이 포스트를 통해 배열의 활용 방법을 익히고, 프로젝트에서 적극적으로 활용해 보세요!
