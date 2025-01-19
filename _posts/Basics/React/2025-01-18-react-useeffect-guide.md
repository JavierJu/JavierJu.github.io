---
title: "React.js useEffect 완벽 가이드: 사용법, 사례, 그리고 주의사항"
excerpt: "React의 useEffect Hook에 대해 자세히 알아봅니다. 기본 사용법부터 의존성 배열 관리, 다양한 실전 사례, 그리고 코드를 최적화하기 위한 팁까지 모두 포함된 가이드입니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, useEffect, Hooks]
permalink: /react/useeffect-guide/
toc: true
toc_sticky: true
date: 2025-01-18
last_modified_at: 2025-01-18
---

## React.js useEffect 완벽 가이드

`useEffect`는 React의 함수형 컴포넌트에서 사이드 이펙트를 처리하기 위한 Hook입니다. 컴포넌트 렌더링 후 API 호출, 이벤트 리스너 추가, 또는 타이머 설정 등의 작업에 사용됩니다. 이 글에서는 `useEffect`의 기본 사용법부터 고급 활용 사례까지 다룹니다.

---

### 기본 사용법
```javascript
import React, { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // 이 코드 블록은 컴포넌트가 렌더링될 때 실행됩니다.
    console.log('Component rendered!');

    return () => {
      // 이 코드 블록은 컴포넌트가 언마운트되거나,
      // 의존성 배열(dependencies)이 변경되기 전에 실행됩니다.
      console.log('Cleanup!');
    };
  }, []);

  return <div>My Component</div>;
}
```

---

### `useEffect`의 주요 특징

#### 1. **렌더링 이후 실행**
- `useEffect` 안의 코드는 컴포넌트가 DOM에 렌더링된 이후에 실행됩니다.
- 렌더링 과정에 영향을 주지 않기 때문에 성능에 미치는 영향을 최소화할 수 있습니다.

#### 2. **의존성 배열 (Dependency Array)**
- `useEffect`의 두 번째 매개변수로 의존성 배열을 전달합니다.
- 배열에 포함된 값이 변경될 때만 `useEffect`가 실행됩니다.

```javascript
useEffect(() => {
  console.log('count가 변경될 때만 실행됩니다!');
}, [count]);
```

#### 3. **의존성 배열 생략**
- 의존성 배열을 생략하면 `useEffect`는 **모든 렌더링 이후마다** 실행됩니다.

```javascript
useEffect(() => {
  console.log('모든 렌더링 후 실행');
});
```

#### 4. **클린업 함수**
- `useEffect`는 정리(clean-up) 작업을 처리할 수 있도록 반환 값을 함수로 받을 수 있습니다.
- 주로 구독 해제, 타이머 제거, DOM 정리 등에 사용됩니다.

```javascript
useEffect(() => {
  const timer = setInterval(() => {
    console.log('Interval running');
  }, 1000);

  return () => clearInterval(timer); // 타이머 정리
}, []);
```

---

### 다양한 활용 사례

#### 1. **API 데이터 가져오기**
```javascript
import React, { useEffect, useState } from 'react';

function FetchDataComponent() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      const response = await fetch('https://api.example.com/data');
      const result = await response.json();
      setData(result);
    }

    fetchData();
  }, []); // 빈 배열이므로 컴포넌트 마운트 시 1회만 실행

  return <div>{data ? JSON.stringify(data) : 'Loading...'}</div>;
}
```

#### 2. **이벤트 리스너 추가 및 제거**
```javascript
import React, { useEffect } from 'react';

function ResizeComponent() {
  useEffect(() => {
    const handleResize = () => {
      console.log('Window resized:', window.innerWidth);
    };

    window.addEventListener('resize', handleResize);

    return () => {
      window.removeEventListener('resize', handleResize); // 이벤트 리스너 제거
    };
  }, []);

  return <div>Resize the window and check the console!</div>;
}
```

#### 3. **타이머 사용**
```javascript
import React, { useState, useEffect } from 'react';

function TimerComponent() {
  const [seconds, setSeconds] = useState(0);

  useEffect(() => {
    const timer = setInterval(() => {
      setSeconds((prev) => prev + 1);
    }, 1000);

    return () => clearInterval(timer); // 타이머 정리
  }, []);

  return <div>Elapsed time: {seconds} seconds</div>;
}
```

---

### `useEffect`의 주의 사항

#### 1. **의존성 배열 관리**
- 의존성 배열에 모든 관련 상태나 props를 포함해야 합니다.
- 의존성을 누락하면 React가 경고를 출력합니다.
- `eslint-plugin-react-hooks`를 사용하면 올바른 의존성을 자동으로 확인할 수 있습니다.

```javascript
useEffect(() => {
  console.log(data);
}, [data]); // data가 변경될 때만 실행
```

#### 2. **무한 루프 방지**
- 의존성 배열을 잘못 설정하면 `useEffect`가 무한 루프에 빠질 수 있습니다.

```javascript
useEffect(() => {
  setCount(count + 1); // 잘못된 사용으로 무한 루프 발생
}, [count]);
```

#### 3. **비동기 함수와의 조합**
- `useEffect` 내에서는 비동기 함수를 직접 호출하지 않는 것이 좋습니다. 대신, 내부에 비동기 함수를 정의하고 호출하세요.

```javascript
useEffect(() => {
  async function fetchData() {
    const response = await fetch('/api/data');
    const data = await response.json();
    console.log(data);
  }

  fetchData();
}, []);
```

---

`useEffect`는 React의 강력한 기능 중 하나로, 컴포넌트의 생명주기(마운트, 업데이트, 언마운트)를 다룰 수 있습니다. 의존성을 잘 관리하면 유지보수가 쉽고 성능이 좋은 코드를 작성할 수 있습니다. 😊

