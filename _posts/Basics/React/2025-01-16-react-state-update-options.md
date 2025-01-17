---
title: "React 상태 업데이트: setState의 함수에 전달하는 값의 형태에 따른 두 가지 사용법"
excerpt: "React의 상태 업데이트 방식인 setState에서 두 가지 접근법의 차이를 비교하고, 콜백 함수의 장점을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 업데이트, 콜백 함수]
permalink: /react/state-update-options/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

React에서 상태를 업데이트할 때 `setState` 함수에는 두 가지 접근 방식이 있습니다. 이 글에서는 다음 두 가지 방식을 비교하고, 콜백 함수 방식이 상태 업데이트에서 더 안전하고 효율적인 이유를 설명합니다.

### 두 가지 상태 업데이트 방식
아래는 간단한 React 컴포넌트 코드 예제입니다. 이 컴포넌트는 버튼을 클릭하면 이모지가 추가되는 기능을 제공합니다.

```jsx
import { useState } from "react";

export default function EmojiClicker() {
    const [emojis, setEmojis] = useState(["😊"]);

    // Option 1
    const addEmoji = () => {
        setEmojis([...emojis, "😊"]);
    };

    // Option 2
    const addEmoji = () => {
        setEmojis((oldEmojis) => [...oldEmojis, "😊"]);
    };

    return (
        <div>
            {emojis.map((e, index) => (
                <span key={index} style={{ fontSize: "4rem" }}>{e}</span>
            ))}
            <button onClick={addEmoji}>Add Emoji</button>
        </div>
    );
}
```

두 옵션의 차이점은 `setEmojis` 함수에 전달하는 값의 형태에 있습니다.

---

### Option 1: 배열 스프레드 사용
```javascript
const addEmoji = () => {
    setEmojis([...emojis, "😊"]);
};
```
- 현재 상태값 `emojis`를 읽어서 새 배열을 생성한 뒤, 이를 `setEmojis`에 전달합니다.
- 문제점: React 상태 업데이트는 **비동기적**으로 동작하므로 상태가 오래된 값일 가능성이 있습니다.
- 예를 들어, 버튼을 빠르게 여러 번 클릭하면 상태가 정확히 업데이트되지 않을 수 있습니다.

---

### Option 2: 콜백 함수 사용
```javascript
const addEmoji = () => {
    setEmojis((oldEmojis) => [...oldEmojis, "😊"]);
};
```
- `setEmojis`에 콜백 함수를 전달합니다.
- React는 항상 최신 상태값 `oldEmojis`를 콜백 함수의 인자로 제공하므로, 상태 업데이트의 정확성을 보장합니다.
- 이 방식은 비동기적인 상태 업데이트 과정에서도 안정적으로 동작합니다.

---

### 왜 Option 2가 더 안전한가?
React 상태 업데이트는 비동기로 예약됩니다. 따라서 상태를 연속적으로 업데이트해야 하는 경우, Option 1처럼 이전 상태값을 직접 참조하면 최신 상태를 놓칠 가능성이 있습니다.

#### Option 1 문제 상황 예제
버튼을 빠르게 3번 클릭하면:
1. `emojis`는 초기값이 `["😊"]`입니다.
2. 첫 번째 클릭: `["😊"]`에서 `setEmojis([...emojis, "😊"])` 실행 → 결과는 `["😊", "😊"]`.
3. 두 번째 클릭: 이전 `setEmojis`가 완료되지 않아 여전히 `["😊"]` 상태로 동작 → 결과는 `["😊", "😊"]`.
4. 세 번째 클릭: 같은 문제 반복 → 최종 결과 `["😊", "😊"]`.

#### Option 2 정상 동작
Option 2는 항상 최신 상태를 기준으로 업데이트합니다. 버튼을 빠르게 3번 클릭하면:
1. `setEmojis((oldEmojis) => [...oldEmojis, "😊"])` 실행.
2. `oldEmojis`는 React가 제공하는 최신 상태값을 보장.
3. 클릭 순서대로 `["😊"] → ["😊", "😊"] → ["😊", "😊", "😊"]`로 정확히 업데이트.

---

### 결론
Option 2처럼 콜백 함수를 사용하는 방식은 상태 업데이트가 연속적으로 발생하거나 비동기적인 상황에서도 안전하게 동작합니다. React에서 상태를 업데이트할 때는 콜백 함수를 사용하는 습관을 들이는 것이 좋습니다.

### 참고
- [React 공식 문서: 상태 업데이트](https://react.dev/learn/updating-state)

