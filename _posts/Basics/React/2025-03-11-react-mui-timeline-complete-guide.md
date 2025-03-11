---
title: "MUI Timeline 컴포넌트 완전 정복: 구성 요소, 사용법, 예제까지"
excerpt: "MUI(Material-UI)의 Timeline 컴포넌트를 활용해 시간 기반 데이터를 시각화하는 방법을 자세히 설명합니다. 구성 요소, 사용법, 예제 코드를 통해 실전 적용까지 익혀보세요."
categories:
  - React
  - Frontend
  - MUI
  - UI Component

tags:
  - [MUI, React, Timeline, UI, 시각화]
permalink: /react/mui-timeline-complete-guide/
toc: true
toc_sticky: true
date: 2025-03-11
last_modified_at: 2025-03-11
---

MUI(Material-UI)의 Timeline 컴포넌트는 시간 순서대로 이벤트나 활동을 시각적으로 표현할 수 있도록 설계된 UI 도구입니다. 연혁, 프로젝트 진행 상황, 일정 등을 직관적으로 보여주고자 할 때 매우 유용합니다.

## 📌 핵심 구성 요소

### 1. `Timeline`

전체 타임라인을 감싸는 컨테이너 역할을 합니다.

### 2. `TimelineItem`

각각의 이벤트를 나타내는 블록입니다.

### 3. `TimelineSeparator`

점(`TimelineDot`)과 연결선(`TimelineConnector`)을 포함하는 구분자 역할입니다.

### 4. `TimelineDot`

타임라인의 이벤트를 시각적으로 표현하는 아이콘 또는 점입니다.

### 5. `TimelineConnector`

이전 또는 다음 아이템과 선으로 연결하는 역할을 합니다.

### 6. `TimelineContent`

이벤트에 대한 설명이나 내용을 나타냅니다.

### 7. `TimelineOppositeContent`

이벤트 시간이나 추가 정보를 표시하는 공간입니다.

## ⚙️ 설치 방법

`@mui/lab` 패키지를 별도로 설치해야 합니다:

```bash
npm install @mui/lab @mui/material @emotion/react @emotion/styled
```

## 💡 기본 예제 코드

아래는 가장 기본적인 수직 타임라인 예시입니다:

```jsx
import * as React from "react";
import Timeline from "@mui/lab/Timeline";
import TimelineItem from "@mui/lab/TimelineItem";
import TimelineSeparator from "@mui/lab/TimelineSeparator";
import TimelineConnector from "@mui/lab/TimelineConnector";
import TimelineContent from "@mui/lab/TimelineContent";
import TimelineDot from "@mui/lab/TimelineDot";
import TimelineOppositeContent from "@mui/lab/TimelineOppositeContent";

export default function BasicTimeline() {
  return (
    <Timeline>
      <TimelineItem>
        <TimelineOppositeContent>09:30 AM</TimelineOppositeContent>
        <TimelineSeparator>
          <TimelineDot />
          <TimelineConnector />
        </TimelineSeparator>
        <TimelineContent>첫 번째 이벤트</TimelineContent>
      </TimelineItem>

      <TimelineItem>
        <TimelineOppositeContent>10:00 AM</TimelineOppositeContent>
        <TimelineSeparator>
          <TimelineDot />
          <TimelineConnector />
        </TimelineSeparator>
        <TimelineContent>두 번째 이벤트</TimelineContent>
      </TimelineItem>
    </Timeline>
  );
}
```

## 🎨 커스터마이징 팁

- `TimelineDot`에 색상, 아이콘 등을 추가해 더 직관적인 표현 가능
- 수직/수평 레이아웃 선택 가능 (`position="alternate"`, `position="right"` 등)
- 반응형 디자인이 기본 지원됨
- Material-UI의 스타일 시스템(`sx` prop 또는 `styled`)으로 자유로운 커스터마이징 가능

## 🧩 활용 예시

- 개인 포트폴리오에서 경력 또는 프로젝트 타임라인
- 일정 관리 앱에서 이벤트 시간 순 정렬
- 비즈니스 히스토리, 연혁, 로드맵 시각화 등

---

MUI의 Timeline 컴포넌트는 시간 기반 정보를 사용자에게 직관적으로 전달하고 싶은 모든 상황에서 강력한 도구가 됩니다. 위 예제와 팁을 참고하여 여러분의 프로젝트에 자연스럽게 녹여보세요!
