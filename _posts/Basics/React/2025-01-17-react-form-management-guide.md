---
title: "React.js의 Form 완벽 가이드: Controlled와 Uncontrolled, 유효성 검사까지"
excerpt: "React.js에서 Form을 관리하는 방법에 대해 자세히 알아보고, Controlled Components와 Uncontrolled Components, 그리고 유효성 검사와 관련된 코드 예제를 함께 살펴봅니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, Form 관리, Controlled Components, Uncontrolled Components, 유효성 검사]
permalink: /react/form-management-guide/
toc: true
toc_sticky: true
date: 2025-01-17
last_modified_at: 2025-01-17
---

React.js에서 Form은 사용자 입력을 처리하고 데이터와 상호작용하는 데 핵심적인 요소입니다. 이 글에서는 **Controlled Components**와 **Uncontrolled Components**의 차이점, Form 유효성 검사, 그리고 Form 관련 Best Practices와 라이브러리를 소개합니다.

---

## Controlled Components
**Controlled Components**는 React의 `state`를 통해 Form 데이터를 관리하는 방식입니다. 입력값이 상태와 동기화되며, 상태 변화에 따라 UI가 즉각적으로 업데이트됩니다.

### 특징
- 입력값은 React 상태(`state`)와 동기화됩니다.
- `onChange` 이벤트 핸들러를 사용하여 상태를 업데이트합니다.
- React가 Form 값을 "컨트롤"합니다.

### 코드 예제
```jsx
import React, { useState } from 'react';

function ControlledForm() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault(); // 폼 제출 기본 동작 방지
    console.log(`Name: ${name}, Email: ${email}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input
          type="text"
          value={name}
          onChange={(e) => setName(e.target.value)}
        />
      </label>
      <br />
      <label>
        Email:
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default ControlledForm;
```

---

## Uncontrolled Components
**Uncontrolled Components**는 Form 값을 React 상태가 아닌 **DOM**이 관리하는 방식입니다. `ref`를 사용하여 입력 요소의 값을 읽습니다.

### 특징
- 초기값만 React에서 설정하며 이후에는 DOM이 값을 관리합니다.
- `ref`를 사용하여 값에 접근합니다.

### 코드 예제
```jsx
import React, { useRef } from 'react';

function UncontrolledForm() {
  const nameRef = useRef(null);
  const emailRef = useRef(null);

  const handleSubmit = (e) => {
    e.preventDefault();
    console.log(`Name: ${nameRef.current.value}, Email: ${emailRef.current.value}`);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Name:
        <input type="text" ref={nameRef} />
      </label>
      <br />
      <label>
        Email:
        <input type="email" ref={emailRef} />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default UncontrolledForm;
```

---

## Form Validation
Form 데이터를 검증하여 사용자 입력의 유효성을 확인하는 작업은 필수입니다.

### 클라이언트 측 유효성 검사
- 상태 값과 조건문을 활용하여 유효성 검사를 수행합니다.
- 오류 메시지를 사용자에게 제공하여 입력을 수정하도록 유도합니다.

### 코드 예제
```jsx
import React, { useState } from 'react';

function FormValidation() {
  const [email, setEmail] = useState('');
  const [error, setError] = useState('');

  const handleSubmit = (e) => {
    e.preventDefault();
    if (!email.includes('@')) {
      setError('Invalid email address');
      return;
    }
    setError('');
    console.log('Form submitted:', email);
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Email:
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </label>
      {error && <p style={{ color: 'red' }}>{error}</p>}
      <button type="submit">Submit</button>
    </form>
  );
}

export default FormValidation;
```

---

## Form Libraries
복잡한 Form을 다룰 때는 **라이브러리**를 사용하는 것이 효율적입니다. 대표적인 라이브러리는 다음과 같습니다.

### React Hook Form
- 간단하고 성능이 뛰어난 Form 관리 라이브러리입니다.
- Controlled, Uncontrolled 방식을 혼합하여 효율적인 Form을 제공합니다.

#### 코드 예제
```jsx
import React from 'react';
import { useForm } from 'react-hook-form';

function HookForm() {
  const { register, handleSubmit, formState: { errors } } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <label>
        Name:
        <input {...register('name', { required: 'Name is required' })} />
      </label>
      {errors.name && <p>{errors.name.message}</p>}
      <br />
      <label>
        Email:
        <input
          {...register('email', {
            required: 'Email is required',
            pattern: { value: /\S+@\S+\.\S+/, message: 'Invalid email address' }
          })}
        />
      </label>
      {errors.email && <p>{errors.email.message}</p>}
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default HookForm;
```

---

## Best Practices
1. **상태 관리**: 간단한 Form은 Controlled Components를, 복잡한 Form은 라이브러리를 사용하는 것이 효율적입니다.
2. **유효성 검사**: 클라이언트와 서버 측 유효성 검사를 모두 수행하세요.
3. **Ref 활용**: 특정 작업(예: 포커스 이동)에는 `ref`를 활용하세요.
4. **라이브러리 사용**: React Hook Form이나 Formik은 대규모 Form 관리를 단순화합니다.

---

React.js의 Form 관리 방식은 프로젝트의 복잡성과 요구 사항에 따라 선택해야 합니다. 위의 가이드를 참고하여 더 나은 사용자 경험을 제공하는 Form을 구현해 보세요!

