---
title: "React Big Calendar 스타일 커스터마이징과 MUI 연동 방법"
excerpt: "react-big-calendar 라이브러리를 사용해 일정 캘린더를 만들고, MUI(Material UI) 스타일과 조화롭게 통합하는 방법을 코드 예제와 함께 소개합니다. 기본 CSS 커스터마이징부터 MUI 컴포넌트와의 조합까지 자세히 다룹니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Calendar, MUI, 스타일 커스터마이징]
permalink: /react/big-calendar-mui-style/
toc: true
toc_sticky: true
date: 2025-03-11
last_modified_at: 2025-03-11
---

## 📅 react-big-calendar란?

`react-big-calendar`는 Google Calendar 스타일의 일정 UI를 쉽게 만들 수 있는 React 라이브러리입니다. `월/주/일/agenda` 보기, 이벤트 추가 및 표시 등 다양한 기능을 기본 제공합니다.

## ⚙️ 설치 방법

```bash
npm install react-big-calendar date-fns
```

> `date-fns` 외에도 `moment` 또는 `luxon`을 사용할 수 있습니다.

## 🧪 기본 사용 예제

```jsx
import { Calendar, dateFnsLocalizer } from "react-big-calendar";
import "react-big-calendar/lib/css/react-big-calendar.css";
import { format, parse, startOfWeek, getDay } from "date-fns";
import enUS from "date-fns/locale/en-US";

const locales = {
  "en-US": enUS,
};

const localizer = dateFnsLocalizer({
  format,
  parse,
  startOfWeek,
  getDay,
  locales,
});

const events = [
  {
    title: "회의",
    start: new Date(),
    end: new Date(),
  },
];

function MyCalendar() {
  return (
    <div style={{ height: 500 }}>
      <Calendar
        localizer={localizer}
        events={events}
        startAccessor="start"
        endAccessor="end"
        style={{ height: 500 }}
      />
    </div>
  );
}
```

## 🎨 기본 CSS 임포트의 중요성

```js
import "react-big-calendar/lib/css/react-big-calendar.css";
```

이 코드는 필수입니다. 생략하면 스타일이 적용되지 않아 달력이 깨져 보이게 됩니다.

## 🎨 CSS 커스터마이징 예제

```css
/* index.css 또는 App.css */
.rbc-calendar {
  background-color: #f9f9f9;
  font-family: "Roboto", sans-serif;
}

.rbc-event {
  background-color: #1976d2;
  color: white;
  border: none;
  padding: 4px;
  border-radius: 4px;
  font-size: 0.85rem;
}

.rbc-toolbar {
  background-color: white;
  border-bottom: 1px solid #ddd;
  margin-bottom: 10px;
}

.rbc-today {
  background-color: #e3f2fd;
}
```

## 🌈 MUI와 함께 사용하는 예제

```jsx
import { Calendar, dateFnsLocalizer } from "react-big-calendar";
import "react-big-calendar/lib/css/react-big-calendar.css";
import { format, parse, startOfWeek, getDay } from "date-fns";
import enUS from "date-fns/locale/en-US";
import { Paper, Box, Typography } from "@mui/material";

const locales = {
  "en-US": enUS,
};

const localizer = dateFnsLocalizer({
  format,
  parse,
  startOfWeek,
  getDay,
  locales,
});

const events = [
  {
    title: "MUI 스타일 회의",
    start: new Date(),
    end: new Date(),
  },
];

export default function MyCalendar() {
  return (
    <Paper elevation={3} sx={{ padding: 2 }}>
      <Typography variant="h5" mb={2}>
        내 캘린더
      </Typography>
      <Box sx={{ height: 500 }}>
        <Calendar
          localizer={localizer}
          events={events}
          startAccessor="start"
          endAccessor="end"
          style={{ height: "100%" }}
        />
      </Box>
    </Paper>
  );
}
```

## 🎁 이벤트 스타일도 커스터마이징 가능

```jsx
<Calendar
  ...
  eventPropGetter={(event) => ({
    style: {
      backgroundColor: '#1976d2',
      color: 'white',
      borderRadius: '4px',
      padding: '2px 6px',
      fontSize: '0.8rem',
    },
  })}
/>
```

## ✅ 마무리 팁

- 반응형이 기본 지원되지 않으므로 직접 스타일 조절 필요
- 모바일 대응이나 일정 드래그 기능은 추가 설정 필요 (`react-dnd` 활용 등)
- MUI 컴포넌트와 함께 사용하면 디자인 일관성을 유지하면서도 기능적인 캘린더 구현 가능

---

다음 포스팅에서는 `이벤트 생성 및 수정`, `드래그 앤 드롭`, `백엔드 연동` 등 고급 기능도 소개할 예정입니다! 🙌
