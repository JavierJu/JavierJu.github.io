---
title: "React MUI에서 SvgIcon 사용법 완벽 정리"
excerpt: "React 프로젝트에서 MUI의 SvgIcon 컴포넌트를 활용해 커스텀 SVG 아이콘을 만드는 방법을 코드 예제와 함께 자세히 설명합니다."
categories:
  - React
  - MUI
  - Frontend
tags:
  - [React, MUI, SvgIcon, 커스텀 아이콘, 프론트엔드]
permalink: /react/mui-svgicon-guide/
toc: true
toc_sticky: true
date: 2025-03-11
last_modified_at: 2025-03-11
---

## ✅ SvgIcon이란?

`SvgIcon`은 React에서 SVG 이미지를 **MUI 스타일 시스템**과 함께 사용할 수 있게 해주는 컴포넌트입니다. 기본 Material Icons 외에 **디자인팀에서 제공한 SVG**나 외부에서 가져온 SVG를 커스터마이징하여 사용하고 싶을 때 유용합니다.

---

## ✅ 기본 사용 방법

```jsx
import SvgIcon from "@mui/material/SvgIcon";

function HomeIcon(props) {
  return (
    <SvgIcon {...props}>
      <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z" />
    </SvgIcon>
  );
}
```

- `SvgIcon`은 기본적으로 `viewBox="0 0 24 24"` 설정이 포함되어 있으며, MUI 시스템 색상과 크기 적용이 가능합니다.

### 사용 예시

```jsx
<HomeIcon color="primary" fontSize="large" />
```

---

## ✅ 외부 SVG 파일을 SvgIcon으로 변환하기

Figma 등에서 export한 SVG 파일을 사용할 수 있습니다.

### SVG 예시 코드:

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
  <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z"/>
</svg>
```

### React SvgIcon으로 변환:

```jsx
import SvgIcon from "@mui/material/SvgIcon";

function CustomCircleIcon(props) {
  return (
    <SvgIcon {...props} viewBox="0 0 24 24">
      <path d="M12 2C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2z" />
    </SvgIcon>
  );
}
```

---

## ✅ 기본 아이콘 vs 커스텀 SvgIcon

| 항목                  | 설명                                                   |
| --------------------- | ------------------------------------------------------ |
| `@mui/icons-material` | MUI가 제공하는 기본 아이콘. 바로 import해서 사용 가능. |
| `SvgIcon`             | path를 직접 정의하여 사용하는 커스텀 아이콘 컴포넌트.  |

---

## ✅ TypeScript에서 사용하기

```tsx
import { SvgIcon, SvgIconProps } from "@mui/material";

const MyIcon = (props: SvgIconProps) => (
  <SvgIcon {...props}>
    <path d="..." />
  </SvgIcon>
);
```

---

## ✅ 예제: GitHub 아이콘 만들기

```jsx
import SvgIcon from "@mui/material/SvgIcon";

function GitHubIcon(props) {
  return (
    <SvgIcon {...props}>
      <path d="M12 0C5.37 0 0 5.37 0 12c0 5.3 3.44 9.8 8.2 11.4..." />
    </SvgIcon>
  );
}
```

```jsx
<GitHubIcon color="action" fontSize="small" />
```

---

## ✅ 언제 SvgIcon을 사용해야 할까?

- 디자인팀에서 만든 **고유 아이콘**이 필요한 경우
- SVG 파일을 직접 가져와야 하는 경우
- MUI 테마 색상, 크기, hover 등 스타일 시스템을 그대로 활용하고 싶은 경우

---

## ✅ 마무리

MUI의 `SvgIcon`을 사용하면 커스터마이징된 SVG 아이콘도 MUI 스타일 시스템 안에서 통일감 있게 사용할 수 있습니다.  
외부에서 가져온 아이콘도 간단한 코드 변환만으로 프로젝트에 바로 사용할 수 있어 매우 유용하니, 꼭 익혀두시길 추천합니다! 😎
