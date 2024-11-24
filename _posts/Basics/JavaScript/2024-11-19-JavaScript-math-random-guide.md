---
title: "JavaScript의 Math 객체와 난수 생성 완벽 가이드"
excerpt: "Math 객체를 활용하여 난수를 생성하고 다양한 상황에서 응용하는 방법을 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Math, Random, Coding Techniques, Game Development]
permalink: /JavaScript/math-random-guide/
toc: true
toc_sticky: true
date: 2024-11-19
last_modified_at: 2024-11-19
---

JavaScript에서 난수를 생성하거나 수학 연산을 처리할 때 자주 사용하는 `Math` 객체는 다양한 메서드와 상수를 제공하여 개발자의 작업을 단순화합니다. 이번 포스트에서는 Math 객체와 난수 생성에 대해 자세히 알아보고, 실제 활용 예제와 함께 다양한 상황에서의 응용 방법을 소개합니다.

## Math 객체란?

`Math` 객체는 JavaScript에 내장된 정적 객체로, 수학적 계산에 필요한 여러 유용한 메서드와 상수를 제공합니다. 이 객체는 인스턴스를 생성할 필요 없이 바로 사용할 수 있습니다.

### 주요 메서드 및 속성
1. **`Math.random()`**: 0 이상 1 미만의 난수를 반환합니다.
2. **`Math.round(x)`**: x를 반올림한 값을 반환합니다.
3. **`Math.floor(x)`**: x를 내림한 값을 반환합니다.
4. **`Math.ceil(x)`**: x를 올림한 값을 반환합니다.
5. **`Math.max(a, b, ...)`**: 전달된 값 중 가장 큰 값을 반환합니다.
6. **`Math.min(a, b, ...)`**: 전달된 값 중 가장 작은 값을 반환합니다.
7. **`Math.pow(base, exponent)`**: base의 exponent 제곱 값을 반환합니다.
8. **`Math.sqrt(x)`**: x의 제곱근을 반환합니다.
9. **`Math.abs(x)`**: x의 절대값을 반환합니다.
10. **`Math.PI`**: 원주율(π, 약 3.14159)을 제공합니다.

## 난수 생성

JavaScript에서 난수를 생성하기 위해 `Math.random()`을 사용합니다. 이를 기반으로 특정 범위의 난수를 생성하거나, 정수 난수를 만들어 다양한 응용에 활용할 수 있습니다.

### 1. 0~1 사이의 난수
```js
let randomNum = Math.random();
console.log(randomNum); // 0 ≤ randomNum < 1
```

### 2. 정수 난수 (0~n)
```js
// 0부터 9까지의 정수 난수
let randomInt = Math.floor(Math.random() * 10);
console.log(randomInt); // 0 ≤ randomInt ≤ 9
```

### 3. 특정 범위의 정수 난수
```js
function getRandomInt(min, max) {
    // min 이상, max 이하의 정수 반환
    return Math.floor(Math.random() * (max - min + 1)) + min;
}
console.log(getRandomInt(1, 100)); // 1 ≤ x ≤ 100
```

## 난수의 활용 예제

### 1. 주사위 굴리기
```js
let dice = Math.floor(Math.random() * 6) + 1; // 1~6
console.log(`주사위 결과: ${dice}`);
```

### 2. 배열에서 랜덤 요소 선택
```js
let fruits = ['apple', 'banana', 'cherry', 'date'];
let randomFruit = fruits[Math.floor(Math.random() * fruits.length)];
console.log(randomFruit);
```

### 3. 랜덤 색상 생성
```js
function getRandomColor() {
    let r = Math.floor(Math.random() * 256);
    let g = Math.floor(Math.random() * 256);
    let b = Math.floor(Math.random() * 256);
    return `rgb(${r}, ${g}, ${b})`;
}
console.log(getRandomColor()); // 예: rgb(123, 45, 67)
```

### 4. 배열 섞기 (Shuffle)
```js
function shuffleArray(array) {
    for (let i = array.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [array[i], array[j]] = [array[j], array[i]]; // Swap
    }
    return array;
}
let numbers = [1, 2, 3, 4, 5];
console.log(shuffleArray(numbers)); // 배열 섞기
```

## 마무리

Math 객체와 난수 생성은 게임 개발, 데이터 샘플링, UI 연출 등 다양한 분야에서 활용됩니다. 이를 활용하면 더 나은 사용자 경험과 효율적인 로직을 구현할 수 있습니다. 위의 예제를 직접 따라 해 보면서 활용법을 익혀보세요!
