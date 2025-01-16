---
title: "React로 만든 랜덤 컬러 박스 예제"
excerpt: "React를 활용한 랜덤 컬러 박스 예제 프로젝트를 코드와 함께 상세히 설명합니다. 이 프로젝트는 React의 상태 관리와 이벤트 처리를 학습하기에 좋은 연습 과제입니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, Frontend, 상태 관리, 이벤트 처리]
permalink: /react/random-color-boxes/
toc: true
toc_sticky: true
date: 2025-01-15
last_modified_at: 2025-01-15
---

이 예제에서는 React를 이용하여 여러 개의 색상 박스를 생성하고, 사용자가 박스를 클릭할 때마다 색상이 랜덤으로 변경되는 기능을 구현했습니다. 이 프로젝트는 React의 `useState`와 `onClick` 이벤트 핸들러를 활용한 간단한 예제로, React 컴포넌트 간의 데이터 전달과 상태 관리 방법을 연습할 수 있습니다.

## 1. 프로젝트 구조

- **App.jsx**: 앱의 루트 컴포넌트로, `ColorBoxes` 컴포넌트를 렌더링합니다.
- **ColorBoxes.jsx**: 여러 개의 `ColorBox` 컴포넌트를 포함하는 컨테이너 컴포넌트입니다.
- **ColorBox.jsx**: 색상을 가진 박스를 랜덤으로 표시하고, 클릭 시 색상이 변경되는 컴포넌트입니다.
- **ColorBox.css**: `ColorBox` 컴포넌트의 스타일을 정의합니다.
- **ColorBoxes.css**: `ColorBoxes` 컴포넌트의 스타일을 정의합니다.

## 2. 주요 코드 설명

### App.jsx

```jsx
import './App.css';
import ColorBoxes from './ColorBoxes';

function App() {
  return (
    <div>
      <ColorBoxes colors={[
        "#E53935", "#AB47BC", "#1E88E5", "#43A047", "#FF7043", "#FFEB3B", "#8E24AA", "#3949AB", "#FBC02D", "#29B6F6", "#8BC34A", "#CDDC39", "#9C27B0", "#FF4081", "#01579B", "#F44336", "#4CAF50", "#FF9800", "#9E9D24", "#FF5722"
      ]} />
    </div>
  );
}

export default App;
```

`ColorBoxes` 컴포넌트를 렌더링하며, 20개 이상의 색상 배열을 `colors` props로 전달합니다.

### ColorBox.jsx

```jsx
import { useState } from 'react';
import "./ColorBox.css"

function randomChois(arr) {
    const idx = Math.floor(Math.random() * arr.length)
    return arr[idx];
}

export default function ColorBox({ colors }) {
    const [color, setColor] = useState(randomChois(colors));

    const result = () => {
        const randomColor = randomChois(colors);
        setColor(randomColor);
    };

    return (
        <div
            className='ColorBox'
            style={{ backgroundColor: color }}
            onClick={result}
        ></div>
    );
}
```

- `randomChois` 함수는 배열에서 무작위로 색상을 선택하는 함수입니다.
- `useState`를 사용하여 `color` 상태를 관리하며, `onClick` 이벤트를 통해 색상을 변경합니다.

### ColorBoxes.jsx

```jsx
import "./ColorBoxes.css"
import ColorBox from "./ColorBox"

export default function ColorBoxes({ colors }) {
    const boxes = [];
    for (let i = 0; i < 25; i++) {
        boxes.push(<ColorBox colors={colors} />);
    }

    return (
        <div className="ColorBoxes">
            {boxes}
        </div>
    );
}
```

- `ColorBoxes` 컴포넌트는 25개의 `ColorBox` 컴포넌트를 생성하고, 각 `ColorBox`에 동일한 `colors` 배열을 전달합니다.

### ColorBox.css

```css
.ColorBox {
    width: 20%;
    height: 20%;
}
```

- 각 `ColorBox`는 20% 크기로 설정하여 화면에 일정 비율로 표시됩니다.

### ColorBoxes.css

```css
.ColorBoxes {
    width: 800px;
    height: 800px;
    border: 1px solid black;
    display: flex;
    flex-wrap: wrap;
}
```

- `ColorBoxes`는 800x800px 크기로 설정되고, `flexbox`를 사용하여 박스를 5x5 형태로 배치합니다.

## 3. 작동 방식

1. `App.jsx`에서 `ColorBoxes` 컴포넌트를 렌더링하며, 여러 색상 코드가 담긴 배열을 전달합니다.
2. `ColorBoxes.jsx`는 25개의 `ColorBox` 컴포넌트를 생성합니다.
3. 각 `ColorBox`는 랜덤 색상으로 초기화되며, 사용자가 클릭할 때마다 색상이 랜덤으로 변경됩니다.

## 4. 실행 방법

이 예제는 React 환경에서 실행할 수 있습니다. 다음 명령어로 프로젝트를 시작할 수 있습니다.

```bash
npm install
npm start
```

