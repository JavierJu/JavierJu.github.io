---
title: "JavaScript 문자열 완벽 가이드: 기본부터 메서드까지"
excerpt: "JavaScript 문자열에 대한 모든 것을 알아봅니다. 기본 개념부터 메서드 활용, 템플릿 리터럴과 유니코드 처리까지 자세히 다뤄봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, String, Web Development, Programming]
permalink: /JavaScript/complete-string-guide/
toc: true
toc_sticky: true
date: 2024-11-18
last_modified_at: 2024-11-18
---

JavaScript에서 문자열은 텍스트 데이터를 표현하는 기본 데이터 타입입니다. 문자열 처리의 기본부터 다양한 메서드와 고급 기능까지 자세히 알아봅시다.

---

## **문자열 기본**
JavaScript에서 문자열은 큰따옴표(`"`), 작은따옴표(`'`), 백틱(```` ```)으로 감싸서 정의합니다.

```js
let singleQuote = 'Hello, World!';
let doubleQuote = "Hello, World!";
let templateLiteral = `Hello, World!`;
```

문자열은 유니코드 문자로 구성되어 다양한 언어와 심볼을 포함할 수 있습니다.

```js
let unicodeExample = "안녕하세요 🌟";
```

---

## **문자열 속성**

### **1. length**
문자열의 길이를 반환합니다.

```js
let str = "Hello";
console.log(str.length); // 5
```

---

## **문자열 메서드**

### **1. 문자열 검색**
#### **indexOf()**
특정 문자열이 처음 등장하는 위치를 반환하며, 찾지 못하면 `-1`을 반환합니다.

```js
let str = "Hello, World!";
console.log(str.indexOf("World")); // 7
console.log(str.indexOf("world")); // -1
```

#### **lastIndexOf()**
문자열 내에서 마지막으로 등장하는 위치를 반환합니다.

```js
let str = "Hello, World, World!";
console.log(str.lastIndexOf("World")); // 13
```

#### **includes()**
문자열에 특정 텍스트가 포함되어 있는지 확인합니다.

```js
let str = "Hello, World!";
console.log(str.includes("World")); // true
console.log(str.includes("world")); // false
```

#### **startsWith() / endsWith()**
문자열이 특정 텍스트로 시작하거나 끝나는지 확인합니다.

```js
let str = "Hello, World!";
console.log(str.startsWith("Hello")); // true
console.log(str.endsWith("World!")); // true
```

---

### **2. 문자열 추출**
#### **slice(start, end)**
지정한 `start`와 `end` 인덱스 사이의 문자열을 반환합니다.

```js
let str = "Hello, World!";
console.log(str.slice(0, 5)); // "Hello"
console.log(str.slice(-6));  // "World!"
```

#### **substring(start, end)**
`slice`와 비슷하지만 음수 인덱스를 지원하지 않습니다.

```js
let str = "Hello, World!";
console.log(str.substring(0, 5)); // "Hello"
console.log(str.substring(5, 0)); // "Hello"
```

#### **substr(start, length)**  
현재는 사용이 권장되지 않으나, 길이를 기반으로 문자열을 추출합니다.

```js
let str = "Hello, World!";
console.log(str.substr(7, 5)); // "World"
```

---

### **3. 문자열 변환**
#### **toUpperCase() / toLowerCase()**
대소문자를 변환합니다.

```js
let str = "Hello, World!";
console.log(str.toUpperCase()); // "HELLO, WORLD!"
console.log(str.toLowerCase()); // "hello, world!"
```

#### **trim()**
문자열의 양쪽 공백을 제거합니다.

```js
let str = "   Hello, World!   ";
console.log(str.trim()); // "Hello, World!"
```

#### **padStart() / padEnd()**
문자열 앞/뒤를 특정 문자로 채웁니다.

```js
let str = "5";
console.log(str.padStart(3, "0")); // "005"
console.log(str.padEnd(3, "0"));   // "500"
```

#### **repeat()**
문자열을 지정된 횟수만큼 반복합니다.

```js
let str = "Ha";
console.log(str.repeat(3)); // "HaHaHa"
```

---

### **4. 문자열 대체**
#### **replace()**
첫 번째로 일치하는 문자열을 대체합니다.

```js
let str = "Hello, World!";
console.log(str.replace("World", "JavaScript")); // "Hello, JavaScript!"
```

#### **replaceAll()**
일치하는 모든 문자열을 대체합니다.

```js
let str = "Apple, Banana, Apple";
console.log(str.replaceAll("Apple", "Orange")); // "Orange, Banana, Orange"
```

---

### **5. 문자열 분할과 결합**
#### **split(separator, limit)**
문자열을 구분자로 나누어 배열로 반환합니다.

```js
let str = "Apple, Banana, Cherry";
console.log(str.split(", ")); // ["Apple", "Banana", "Cherry"]
```

#### **concat()**
문자열을 결합합니다.

```js
let str1 = "Hello";
let str2 = "World";
console.log(str1.concat(", ", str2, "!")); // "Hello, World!"
```

---

## **템플릿 리터럴**
백틱(```` ```)을 사용하여 텍스트와 변수를 결합하거나 여러 줄 문자열을 작성할 수 있습니다.

```js
let name = "Alice";
let age = 25;

let greeting = `Hello, my name is ${name}.
I am ${age} years old.`;
console.log(greeting);
```

---

## **유니코드와 이모지 처리**
JavaScript 문자열은 유니코드와 이모지를 지원합니다.

```js
let str = "💻🌟";
console.log(str.length); // 2
console.log(str.charCodeAt(0)); // 128187
```

---

JavaScript 문자열은 다양한 기능과 메서드를 통해 강력한 텍스트 처리를 지원합니다. 이를 활용하여 더 효율적인 웹 애플리케이션을 만들어 보세요!
