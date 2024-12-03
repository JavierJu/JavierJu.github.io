---
title: "JavaScript의 setTimeout과 setInterval의 차이점과 활용"
excerpt: "JavaScript에서 `setTimeout`과 `setInterval`의 차이점과 활용법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, setTimeout, setInterval, Timer, Web Development]
permalink: /JavaScript/setTimeout-setInterval/
toc: true
toc_sticky: true
date: 2024-11-26
last_modified_at: 2024-11-26
---

## `setTimeout`과 `setInterval`의 차이점과 활용

JavaScript에서 `setTimeout`과 `setInterval`은 시간 기반 작업을 예약하는 데 사용되는 중요한 함수입니다. 이 두 함수는 비슷한 목적을 가지고 있지만 동작 방식에 차이가 있으며, 각자의 활용 사례가 다릅니다. 이 글에서는 각 함수의 동작 방식과 차이점, 그리고 실제 사용 예시를 다룹니다.

### `setTimeout`

`setTimeout`은 지정된 시간이 지난 후에 **한 번만** 특정 작업을 실행하는 함수입니다.

#### 구문
```js
let timeoutID = setTimeout(callback, delay, [arg1, arg2, ...]);
```

- **`callback`**: 실행할 함수.
- **`delay`**: 실행까지 기다릴 시간(밀리초 단위).
- **`arg1, arg2, ...`**: 콜백 함수에 전달할 추가 매개변수(선택).

#### 특징
1. **한 번만 실행**: 지정된 시간이 지나면 콜백 함수가 호출되고 작업이 끝납니다.
2. **취소 가능**: `clearTimeout(timeoutID)`로 예약된 작업을 취소할 수 있습니다.

#### 예제
```js
js
function greet(name) {
    console.log(`Hello, ${name}!`);
}

// 2초 후에 한 번 실행
let timer = setTimeout(greet, 2000, 'Alice');

// setTimeout 취소
clearTimeout(timer); // greet 함수는 실행되지 않음
```
### `setInterval`

`setInterval`은 일정한 시간 간격으로 **반복적으로** 특정 작업을 실행하는 함수입니다.

#### 구문
```js
js
let intervalID = setInterval(callback, delay, [arg1, arg2, ...]);
```

- **`callback`**: 실행할 함수.
- **`delay`**: 실행 간격(밀리초 단위).
- **`arg1, arg2, ...`**: 콜백 함수에 전달할 추가 매개변수(선택).

#### 특징
1. **반복 실행**: 지정된 시간 간격마다 콜백 함수가 반복 실행됩니다.
2. **취소 가능**: `clearInterval(intervalID)`로 반복 작업을 중단할 수 있습니다.

#### 예제
```js
let count = 0;

function incrementCounter() {
    count++;
    console.log(`Counter: ${count}`);
    if (count === 5) {
        clearInterval(intervalID); // 5회 실행 후 중단
    }
}

// 1초마다 실행
let intervalID = setInterval(incrementCounter, 1000);
```
### `setTimeout`과 `setInterval`의 차이점

| **특징**              | **setTimeout**                      | **setInterval**                     |
|-----------------------|-------------------------------------|-------------------------------------|
| **동작**              | 지정된 시간 후 한 번 실행           | 지정된 시간마다 반복 실행           |
| **취소 방법**         | `clearTimeout`                     | `clearInterval`                    |
| **일반적 사용 사례**  | 지연 실행, 특정 조건 후 실행        | 주기적인 작업, 타이머 기능         |

### 중첩 `setTimeout`을 통한 `setInterval` 대체

`setInterval` 대신 `setTimeout`을 재귀적으로 호출하여 유사한 반복 작업을 구현할 수 있습니다. 이 방식은 작업 간 시간이 정확히 일정하지 않아도 되는 경우에 유용합니다.

#### 예제
```js
function repeatedTask() {
    console.log('Repeated task is running...');
    setTimeout(repeatedTask, 1000); // 1초 간격으로 반복 실행
}

// 처음 실행
setTimeout(repeatedTask, 1000);
```
### 주의사항

1. **정확성 문제**:
   JavaScript는 싱글 스레드에서 실행되므로 `setTimeout`과 `setInterval`의 실행 시간이 정확하지 않을 수 있습니다. 다른 작업이나 코드가 실행 중이면 지연이 발생할 수 있습니다.
   
2. **중단 잊지 않기**:
   반복 작업(`setInterval`)은 꼭 필요 없을 때 `clearInterval`로 중단하지 않으면 메모리 누수나 예기치 않은 동작이 발생할 수 있습니다.

### 실제 사용 사례

- **setTimeout**:
  - 애니메이션 효과에 약간의 지연 추가.
  - 일정 시간 후 사용자 알림 표시.

- **setInterval**:
  - 시계, 타이머, 주기적 데이터 업데이트.
  - 게임 루프나 애니메이션 프레임 계산.

필요에 따라 두 함수 중 적합한 것을 선택해 사용하면 됩니다! 😊
