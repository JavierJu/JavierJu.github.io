---
title: "@mui/x-date-pickers 완전 정복: 설치부터 실전 사용법까지"
excerpt: "MUI의 날짜 및 시간 선택기 패키지인 @mui/x-date-pickers의 설치, 주요 컴포넌트, 사용 예제, 팁까지 실전 프로젝트에 바로 적용할 수 있도록 정리했습니다."
categories:
  - React
  - Frontend
  - MUI
  - UI Components
tags:
  - [React, JavaScript, MUI, DatePicker, 시간 선택기, UI 컴포넌트]
permalink: /react/mui-x-date-pickers-guide/
toc: true
toc_sticky: true
date: 2025-03-11
last_modified_at: 2025-03-11
---

## @mui/x-date-pickers란?

`@mui/x-date-pickers`는 [MUI(Material UI)]에서 제공하는 **날짜 및 시간 선택기(Date and Time Pickers)** 전용 컴포넌트 모음입니다. 이전에는 `@mui/lab`에 포함되어 있었지만, 현재는 독립된 패키지로 분리되어 더 강력하고 안정적인 기능을 제공합니다.

---

## 🔧 설치 방법

```bash
npm install @mui/x-date-pickers
```

또한 날짜 처리 라이브러리(`dayjs`, `date-fns`, `luxon`, `moment`) 중 하나도 함께 설치해야 합니다. `dayjs`를 기준으로 예시를 들겠습니다:

```bash
npm install dayjs
```

MUI와 date picker를 연결해주는 **date adapter**도 필요합니다:

```bash
npm install @mui/x-date-pickers/AdapterDayjs
```

---

## 📅 주요 컴포넌트 소개

| 컴포넌트                                | 설명                       |
| --------------------------------------- | -------------------------- |
| `DatePicker`                            | 날짜 선택기 (연/월/일)     |
| `TimePicker`                            | 시간 선택기 (시/분)        |
| `DateTimePicker`                        | 날짜 + 시간 선택 가능      |
| `StaticDatePicker`                      | 항상 열려있는 달력 형식    |
| `MobileDatePicker`, `DesktopDatePicker` | 디바이스별 최적화 컴포넌트 |

---

## 🧩 기본 사용 예제

```tsx
import * as React from "react";
import { LocalizationProvider } from "@mui/x-date-pickers/LocalizationProvider";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";
import { DatePicker } from "@mui/x-date-pickers/DatePicker";
import TextField from "@mui/material/TextField";
import dayjs, { Dayjs } from "dayjs";

export default function BasicDatePicker() {
  const [value, setValue] = React.useState<Dayjs | null>(dayjs());

  return (
    <LocalizationProvider dateAdapter={AdapterDayjs}>
      <DatePicker
        label="날짜 선택"
        value={value}
        onChange={(newValue) => setValue(newValue)}
        renderInput={(params) => <TextField {...params} />}
      />
    </LocalizationProvider>
  );
}
```

---

## 🌍 LocalizationProvider란?

`LocalizationProvider`는 날짜와 시간 데이터를 적절한 포맷과 로케일로 처리하기 위한 **상위 Provider 컴포넌트**입니다. `AdapterDayjs`, `AdapterDateFns` 등을 설정해서 날짜 라이브러리와 MUI 컴포넌트를 연결합니다.

이 컴포넌트는 반드시 `DatePicker`, `TimePicker` 등을 사용할 때 **상위에 존재해야 합니다.**

---

## ⏱ 유용한 Props 예시

```tsx
<DatePicker
  minDate={dayjs().subtract(1, "month")}
  maxDate={dayjs().add(1, "month")}
  disablePast
  inputFormat="YYYY-MM-DD"
/>
```

| Props                           | 설명                           |
| ------------------------------- | ------------------------------ |
| `minDate` / `maxDate`           | 선택 가능 날짜 범위 설정       |
| `disablePast` / `disableFuture` | 과거 또는 미래 날짜 비활성화   |
| `shouldDisableDate`             | 조건에 따라 특정 날짜 비활성화 |
| `inputFormat`                   | 입력 필드의 날짜 포맷 지정     |

---

## 💡 실전 팁

- **다국어 지원**: `dayjs`의 locale 기능을 통해 한국어 등 다양한 언어 지원 가능
- **Form 라이브러리와 연동**: `react-hook-form`이나 `formik`과 함께 사용할 수 있으며 `Controller`로 연동 가능
- **커스텀 렌더링**: `renderInput`을 통해 `TextField` 외의 커스텀 UI 구성 가능

---

## 📝 마무리

`@mui/x-date-pickers`는 복잡한 날짜 입력을 직관적인 UI로 바꿔주는 매우 강력한 도구입니다. 실무 프로젝트에 날짜/시간 관련 기능이 필요할 때 이 라이브러리를 적극적으로 활용해 보세요. 특히 MUI를 기반으로 한 디자인 시스템을 사용 중이라면 완벽한 궁합을 보여줍니다.

추가적으로 `react-hook-form` 연동, `DateTimePicker` 심화 사용법이 궁금하시다면 다음 글도 이어서 확인해보세요!
