---
title: "React.js 폼에서 여러 입력 필드 다루기: 효율적인 상태 관리 방법"
excerpt: "React.js에서 폼의 여러 입력 필드를 효율적으로 관리하는 방법을 코드 예제와 함께 자세히 설명합니다. useState를 사용한 상태 관리 및 동적 필드 업데이트 방법을 알아보세요."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 폼, 상태 관리]
permalink: /react/handling-multiple-inputs/
toc: true
toc_sticky: true
date: 2025-01-17
last_modified_at: 2025-01-17
---

## React.js에서 여러 입력 필드 상태 관리하기

React.js에서 폼을 만들 때 여러 입력 필드를 효율적으로 관리하는 방법은 중요합니다. 특히, `useState`를 활용하여 단일 상태 객체로 여러 필드를 관리하면 코드가 간결해지고 유지보수하기 쉬워집니다. 이번 글에서는 이를 구현하는 방법을 코드 예제와 함께 소개합니다.

---

### 1. 기본 개념
- **상태를 객체로 관리**: 입력 필드의 상태를 단일 객체로 묶어 관리.
- **동적 업데이트**: `name` 속성을 활용하여 특정 필드만 업데이트.
- **공통 이벤트 핸들러 사용**: 모든 필드에 동일한 `onChange` 핸들러를 적용.

---

### 2. 코드 예제

#### 기본 예제
아래는 기본적인 React.js 폼 코드입니다. `useState`를 사용하여 여러 입력 필드를 하나의 상태 객체로 관리합니다.

```jsx
import React, { useState } from "react";

function MultipleInputForm() {
  const [formData, setFormData] = useState({
    username: "",
    email: "",
    password: ""
  });

  // 공통적으로 사용할 onChange 핸들러
  const handleChange = (event) => {
    const { name, value } = event.target;
    setFormData((prevData) => ({
      ...prevData, // 기존의 다른 필드 유지
      [name]: value // name에 해당하는 필드만 업데이트
    }));
  };

  const handleSubmit = (event) => {
    event.preventDefault();
    console.log("Submitted Data:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Username:
        <input
          type="text"
          name="username"
          value={formData.username}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Email:
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Password:
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default MultipleInputForm;
```

---

### 3. 핵심 포인트

1. **객체의 불변성 유지**
   - `setFormData` 호출 시 기존 상태를 스프레드 연산자(`...`)로 복사하여 새로운 객체를 생성.
   - React 상태 관리의 불변성을 유지함으로써 효율적인 렌더링이 가능.

2. **동적인 키 업데이트**
   - `event.target.name`을 키로 사용해 특정 필드만 업데이트할 수 있음.

3. **공통 핸들러 사용**
   - 각 입력 필드에 대해 별도의 핸들러를 작성할 필요가 없음.

---

### 4. 추가 기능

#### 입력 값 검증 추가

입력 값에 대한 검증 로직을 추가하면 사용성 및 데이터 무결성을 높일 수 있습니다.

```jsx
const handleChange = (event) => {
  const { name, value } = event.target;

  if (name === "username" && value.length > 15) {
    alert("Username must be less than 15 characters!");
    return;
  }

  setFormData((prevData) => ({
    ...prevData,
    [name]: value
  }));
};
```

#### 초기 상태 리셋

폼을 초기 상태로 리셋하는 기능도 유용합니다.

```jsx
const resetForm = () => {
  setFormData({
    username: "",
    email: "",
    password: ""
  });
};

// 버튼 추가
<button type="button" onClick={resetForm}>Reset</button>
```

---

### 5. TypeScript 버전

TypeScript를 사용하여 타입을 명시적으로 정의하면 더욱 안전한 코드 작성이 가능합니다.

```tsx
import React, { useState, ChangeEvent, FormEvent } from "react";

interface FormData {
  username: string;
  email: string;
  password: string;
}

function MultipleInputForm() {
  const [formData, setFormData] = useState<FormData>({
    username: "",
    email: "",
    password: ""
  });

  const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    const { name, value } = event.target;
    setFormData((prevData) => ({
      ...prevData,
      [name]: value
    }));
  };

  const handleSubmit = (event: FormEvent) => {
    event.preventDefault();
    console.log("Submitted Data:", formData);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Username:
        <input
          type="text"
          name="username"
          value={formData.username}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Email:
        <input
          type="email"
          name="email"
          value={formData.email}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Password:
        <input
          type="password"
          name="password"
          value={formData.password}
          onChange={handleChange}
        />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default MultipleInputForm;
```

---

### 결론

React.js에서 여러 입력 필드를 관리하는 효율적인 방법은 상태를 객체로 묶어 관리하고, `onChange` 이벤트 핸들러를 공통적으로 사용하는 것입니다. 이를 통해 코드의 가독성과 유지보수성을 높일 수 있습니다. 추가적인 검증 및 리셋 기능을 추가하여 사용자 경험을 더욱 향상시킬 수도 있습니다.

