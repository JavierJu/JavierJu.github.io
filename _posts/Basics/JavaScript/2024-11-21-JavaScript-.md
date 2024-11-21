---
title: "JavaScript로 추측 게임 만들기: 코드 상세 분석"
excerpt: "JavaScript를 사용해 간단한 추측 게임을 구현한 코드를 분석하고 각 부분의 역할과 동작 원리를 설명합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Coding Projects, Beginners, Games]
permalink: /JavaScript/guessing-game/
toc: true
toc_sticky: true
date: 2024-11-21
last_modified_at: 2024-11-21
---

JavaScript를 사용해 간단한 숫자 추측 게임을 만들어 봅시다. 이 코드는 사용자가 설정한 최대값 내에서 임의의 숫자를 생성하고, 사용자가 숫자를 추측하며 맞힐 때까지 진행됩니다. 코드를 분석하며 각 부분의 역할과 동작 원리를 알아보겠습니다.

## 전체 코드
```js
let maximum = parseInt(prompt("Enter the maximum number!"));
while (!maximum) {
    maximum = parseInt(prompt("Enter a valid number!"));
}

const targetNum = Math.floor(Math.random() * maximum) + 1;

let guess = prompt("Enter your first guess! (Type 'q' to quit)");
let attempts = 1;

while (parseInt(guess) !== targetNum) {
    if (guess === 'q') break;
    guess = parseInt(guess);
    if (guess > targetNum) {
        guess = prompt("Too high! Enter a new guess:");
        attempts++;
    } else if (guess < targetNum) {
        guess = prompt("Too low! Enter a new guess:");
        attempts++;
    } else {
        guess = prompt("Invalid guess. Please enter a number or 'q' to quit");
    }
}

if (guess === 'q') {
    console.log("OK, YOU QUIT!")
} else {
    console.log("CONGRATS YOU WIN!")
    console.log(`You got it! It took you ${attempts} guesses`)
}
```

---

## 코드 분석

### 1. **최대값 입력 받기**
```js
let maximum = parseInt(prompt("Enter the maximum number!"));
while (!maximum) {
    maximum = parseInt(prompt("Enter a valid number!"));
}
```
- **목적**: 숫자 추측 게임의 범위를 설정합니다.
- `prompt()`를 통해 사용자가 입력한 값을 문자열로 받고, 이를 `parseInt()`로 정수로 변환합니다.
- 사용자가 유효하지 않은 값을 입력했을 경우 `while (!maximum)` 조건으로 계속 입력을 요청합니다.

---

### 2. **목표 숫자 생성**
```js
const targetNum = Math.floor(Math.random() * maximum) + 1;
```
- **목적**: 1부터 사용자가 지정한 최대값 사이의 임의의 숫자를 생성합니다.
- `Math.random()`은 0 이상 1 미만의 난수를 반환합니다.
- `Math.random() * maximum`으로 범위를 조정하고, `Math.floor()`로 정수화한 뒤 `+1`을 추가해 1부터 최대값 사이의 숫자를 만듭니다.

---

### 3. **첫 번째 추측 입력 받기**
```js
let guess = prompt("Enter your first guess! (Type 'q' to quit)");
let attempts = 1;
```
- **목적**: 사용자로부터 첫 번째 추측 값을 입력받습니다.
- `attempts`는 사용자가 몇 번의 시도를 했는지 카운트하기 위한 변수입니다.

---

### 4. **추측 처리와 반복문**
```js
while (parseInt(guess) !== targetNum) {
    if (guess === 'q') break;
    guess = parseInt(guess);
    if (guess > targetNum) {
        guess = prompt("Too high! Enter a new guess:");
        attempts++;
    } else if (guess < targetNum) {
        guess = prompt("Too low! Enter a new guess:");
        attempts++;
    } else {
        guess = prompt("Invalid guess. Please enter a number or 'q' to quit");
    }
}
```
- **목적**: 사용자가 목표 숫자를 맞출 때까지 반복적으로 입력을 처리합니다.
- `parseInt(guess) !== targetNum` 조건을 통해 현재 추측 값과 목표 숫자를 비교합니다.
- 사용자가 `q`를 입력하면 `break`로 반복문을 종료합니다.
- 입력 값이 목표 숫자보다 크면 "Too high!", 작으면 "Too low!" 메시지를 표시하며, `attempts`를 증가시킵니다.

---

### 5. **게임 종료 처리**
```js
if (guess === 'q') {
    console.log("OK, YOU QUIT!")
} else {
    console.log("CONGRATS YOU WIN!")
    console.log(`You got it! It took you ${attempts} guesses`)
}
```
- **목적**: 게임이 종료된 후 적절한 메시지를 출력합니다.
- 사용자가 `q`를 입력하여 종료한 경우 "OK, YOU QUIT!"를 출력합니다.
- 목표 숫자를 맞춘 경우 축하 메시지와 시도 횟수를 출력합니다.

---

## 개선 사항 제안
1. **유효성 검사 강화**  
   사용자가 숫자가 아닌 문자를 입력했을 때의 처리를 더 정교하게 구현할 수 있습니다.

2. **UI 개선**  
   `alert()`를 사용하여 추측 결과를 사용자에게 더 시각적으로 알릴 수 있습니다.

3. **난이도 추가**  
   최대값 범위를 지정하거나 시도 횟수 제한을 두어 난이도를 설정할 수 있습니다.

---

## 결론
이 코드는 간단한 추측 게임의 흐름을 JavaScript로 잘 구현한 예입니다. 숫자 비교와 입력 처리, 그리고 반복문 활용의 기초를 배우는 데 적합합니다. 여러분도 이 코드를 기반으로 자신만의 기능을 추가하며 발전시켜 보세요!
