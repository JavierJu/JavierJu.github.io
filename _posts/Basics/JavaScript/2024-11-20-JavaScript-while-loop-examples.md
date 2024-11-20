---
title: "JavaScript의 while 루프: 예제와 활용 사례"
excerpt: "JavaScript의 while 루프 구조와 활용 사례를 살펴보고, for 루프와의 차이점도 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Programming, Loops, Coding Basics]
permalink: /JavaScript/while-loop-examples/
toc: true
toc_sticky: true
date: 2024-11-20
last_modified_at: 2024-11-20
---

## while 루프란?

`while` 루프는 특정 조건이 **참(true)**일 때 반복적으로 코드를 실행하는 JavaScript의 반복문입니다. 조건이 `false`로 평가되면 루프가 종료됩니다.

기본 문법은 다음과 같습니다:

```js
while (조건) {
  // 반복적으로 실행할 코드
}
```

조건이 처음부터 `false`라면 루프는 한 번도 실행되지 않습니다. 

---

## 예제 코드

아래는 `while` 루프를 사용하여 숫자 1부터 10까지 출력하는 간단한 예제입니다:

```js
let count = 1;

while (count <= 10) {
  console.log(count);
  count++; // count를 1씩 증가
}
```

---

## 자주 쓰이는 상황

1. **조건 기반 반복**  
   - 반복 횟수가 사전에 정해지지 않은 경우 사용됩니다.  
   - 예: 사용자가 특정 입력을 제공할 때까지 계속 요청하기.

```js
let input;
while (input !== "exit") {
  input = prompt("종료하려면 'exit'를 입력하세요:");
}
```

2. **무한 루프 처리**  
   - 서버에서 데이터를 계속 읽거나 특정 조건이 만족될 때까지 실행하는 프로그램에 활용됩니다.

```js
while (true) {
  // 서버 상태 체크 또는 이벤트 대기
  if (checkServerStatus()) {
    break; // 조건 충족 시 루프 탈출
  }
}
```

---

## for 루프와의 차이점

| **특징**              | **while 루프**                                  | **for 루프**                             |
|-----------------------|-----------------------------------------------|------------------------------------------|
| **사용 목적**          | 조건에 따라 반복, 반복 횟수가 미리 정해지지 않음 | 반복 횟수가 명확한 경우에 적합            |
| **구조적 간결함**      | 조건만 작성, 초기값/증감은 별도로 추가해야 함      | 초기화, 조건, 증감 모두 한 줄에 작성 가능 |

### while 루프 예제
```js
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}
```

### for 루프 예제
```js
for (let i = 0; i < 5; i++) {
  console.log(i);
}
```

---

## 마무리

`while` 루프는 유연한 조건 기반 반복이 필요한 상황에서 유용하며, for 루프에 비해 구조적으로 덜 제한적입니다. 하지만 조건이 잘못 설정되면 **무한 루프**에 빠질 위험이 있으므로 사용 시 주의를 기울여야 합니다.

