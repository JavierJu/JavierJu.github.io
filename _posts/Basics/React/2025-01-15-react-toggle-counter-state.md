---
title: "React 상태와 카운터: 클릭 이벤트로 상태와 카운트 동시 업데이트하기"
excerpt: "React 컴포넌트에서 클릭 이벤트를 통해 상태와 카운트를 동시에 업데이트하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 관리, 이벤트 처리]
permalink: /react/toggle-counter-state/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

React를 사용하다 보면 클릭 이벤트를 통해 상태를 변경하거나 카운트를 업데이트하는 경우가 많습니다. 이번 글에서는 클릭 이벤트를 활용하여 상태와 카운트를 동시에 업데이트하는 방법을 알아보겠습니다.

## 문제 상황
아래와 같은 React 컴포넌트가 있다고 가정해봅시다. 클릭 이벤트가 발생했을 때 상태(`isHappy`)와 카운트(`count`)를 동시에 업데이트하고 싶습니다.

```jsx
import { useState } from 'react';

export default function ToggleCounter() {
    const [isHappy, setIsHappy] = useState(true);
    const [count, setCount] = useState(0);

    const toggleIsHappy = () => setIsHappy(!isHappy);
    const countNum = () => setCount(count + 1);

    return (
        <>
            <p className='Toggler' onClick={toggleIsHappy countNum}>
                {isHappy ? "😊" : "😒"}
            </p>
            <p>{count}</p>
        </>
    );
}
```

위 코드에서 `onClick` 핸들러에 `toggleIsHappy`와 `countNum`을 동시에 전달하려 하지만, 이는 올바른 방식이 아닙니다.

## 해결 방법
두 개의 함수를 하나로 묶어서 사용하는 방식으로 문제를 해결할 수 있습니다. 아래는 수정된 코드입니다.

```jsx
import { useState } from 'react';

export default function ToggleCounter() {
    const [isHappy, setIsHappy] = useState(true);
    const [count, setCount] = useState(0);

    const handleClick = () => {
        setIsHappy(!isHappy);
        setCount(count + 1);
    };

    return (
        <>
            <p className='Toggler' onClick={handleClick}>
                {isHappy ? "😊" : "😒"}
            </p>
            <p>{count}</p>
        </>
    );
}
```

### 변경 사항
1. `onClick={toggleIsHappy countNum}`를 `onClick={handleClick}`로 수정했습니다.
2. 두 개의 상태 업데이트 함수를 하나로 묶은 `handleClick` 함수를 정의했습니다.

### 작동 방식
- `p` 태그를 클릭하면 `handleClick` 함수가 실행됩니다.
- `handleClick`은 `setIsHappy`를 호출해 `isHappy` 상태를 토글하고, `setCount`를 호출해 `count`를 1 증가시킵니다.
- React는 상태 변경을 감지하고 UI를 자동으로 업데이트합니다.

## 결과
위 코드를 실행하면 아래와 같은 결과를 확인할 수 있습니다.

- 😊 아이콘을 클릭할 때마다 😒로 바뀌고, 다시 클릭하면 😊로 돌아옵니다.
- 클릭할 때마다 아래의 숫자가 1씩 증가합니다.

React 상태 관리와 이벤트 처리의 기본 원리를 이해하는 데 도움이 되길 바랍니다!

