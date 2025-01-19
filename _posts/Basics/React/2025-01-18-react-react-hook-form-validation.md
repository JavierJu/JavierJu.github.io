---
title: "React Hook Form으로 유효성 검사를 쉽게 구현하기"
excerpt: "React Hook Form의 사용법과 유효성 검사 설정을 자세히 살펴보고, 실제 코드 예제를 통해 구현 방법을 배워봅니다. Yup과 같은 스키마 기반 라이브러리와의 통합도 소개합니다."
categories:
  - React
  - Frontend
tags:
  - [React, React Hook Form, 유효성 검사, JavaScript, Frontend]
permalink: /react/react-hook-form-validation/
toc: true
toc_sticky: true
date: 2025-01-18
last_modified_at: 2025-01-18
---

## React Hook Form을 활용한 유효성 검사

React Hook Form은 간단하고 효율적인 폼 관리를 제공하는 라이브러리로, 폼 데이터의 상태를 추적하고 유효성 검사를 쉽게 수행할 수 있도록 돕습니다. 이 라이브러리는 React의 **Uncontrolled Components** 방식을 활용하여 퍼포먼스를 최적화하며, React에서 네이티브로 제공하는 `ref`를 적극적으로 사용합니다.

---

## React Hook Form에서 유효성 검사의 기본 구조

React Hook Form에서 유효성 검사는 다음과 같은 방법으로 수행됩니다:
1. `register` 함수로 폼 요소를 등록할 때 유효성 검사 규칙을 설정.
2. 유효성 검사 실패 시 `errors` 객체를 통해 에러 메시지를 관리.

### 기본 사용 예제
```jsx
import React from "react";
import { useForm } from "react-hook-form";

export default function App() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm();

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <div>
        <label>이메일</label>
        <input
          {...register("email", {
            required: "이메일은 필수 입력 항목입니다.",
            pattern: {
              value: /^[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Za-z]{2,4}$/,
              message: "유효한 이메일 형식을 입력해주세요.",
            },
          })}
        />
        {errors.email && <p>{errors.email.message}</p>}
      </div>

      <div>
        <label>비밀번호</label>
        <input
          type="password"
          {...register("password", {
            required: "비밀번호는 필수 입력 항목입니다.",
            minLength: {
              value: 6,
              message: "비밀번호는 최소 6자 이상이어야 합니다.",
            },
          })}
        />
        {errors.password && <p>{errors.password.message}</p>}
      </div>

      <button type="submit">제출</button>
    </form>
  );
}
```

---

## `register` 함수의 옵션

`register` 함수는 폼 필드를 React Hook Form에 등록하고, 다음과 같은 옵션을 통해 유효성 검사를 설정할 수 있습니다:

### 주요 옵션
- **`required`**  
  - 필수 입력 여부를 설정합니다.  
  - 문자열 값을 설정하면 에러 메시지를 제공합니다.

- **`pattern`**  
  - 정규식을 이용해 입력 값을 검사합니다.

- **`minLength` / `maxLength`**  
  - 입력 값의 최소/최대 길이를 설정합니다.

- **`min` / `max`**  
  - 숫자 값의 최소/최대 값을 설정합니다.

- **`validate`**  
  - 사용자 정의 유효성 검사를 위해 함수를 설정합니다.

### 사용자 정의 유효성 검사 예제
```jsx
<input
  {...register("username", {
    required: "유저 이름은 필수입니다.",
    validate: (value) => value !== "admin" || "admin은 사용할 수 없는 이름입니다.",
  })}
/>
```

---

## `errors` 객체

`errors` 객체는 각 필드의 유효성 검사 결과를 포함하며, 에러 메시지와 상태를 관리합니다.

- 특정 필드가 유효하지 않을 경우, `errors[필드명]` 객체가 생성됩니다.
- 에러 메시지는 `errors[필드명].message`에 저장됩니다.

### `errors`를 활용한 조건부 렌더링
```jsx
{errors.email && <p>{errors.email.message}</p>}
{errors.password && <p>{errors.password.message}</p>}
```

---

## `resolver`를 사용한 외부 라이브러리 연동

React Hook Form은 `Yup`, `Zod`와 같은 스키마 기반 유효성 검사 라이브러리와도 쉽게 통합할 수 있습니다.

### `Yup`과 함께 사용하는 예제
```bash
npm install @hookform/resolvers yup
```

```jsx
import React from "react";
import { useForm } from "react-hook-form";
import { yupResolver } from "@hookform/resolvers/yup";
import * as Yup from "yup";

const schema = Yup.object().shape({
  email: Yup.string()
    .email("유효한 이메일 주소를 입력하세요.")
    .required("이메일은 필수입니다."),
  password: Yup.string()
    .min(6, "비밀번호는 최소 6자 이상이어야 합니다.")
    .required("비밀번호는 필수입니다."),
});

export default function App() {
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm({
    resolver: yupResolver(schema),
  });

  const onSubmit = (data) => {
    console.log(data);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("email")} />
      {errors.email && <p>{errors.email.message}</p>}

      <input {...register("password")} />
      {errors.password && <p>{errors.password.message}</p>}

      <button type="submit">제출</button>
    </form>
  );
}
```

---

## 비동기 유효성 검사

React Hook Form은 비동기 유효성 검사를 지원합니다. 이를 위해 `validate` 함수에 비동기 로직을 추가할 수 있습니다.

### 예제
```jsx
<input
  {...register("username", {
    validate: async (value) => {
      const response = await fetch(`/api/validate-username?username=${value}`);
      const data = await response.json();
      return data.isValid || "이미 사용 중인 이름입니다.";
    },
  })}
/>
```

---

React Hook Form은 성능과 유연성을 모두 제공하며, 복잡한 폼 유효성 검사에도 적합합니다. 필요에 따라 옵션을 조합해 강력한 폼을 쉽게 구현할 수 있습니다. 추가로 궁금한 점이 있다면 댓글로 질문해주세요!

