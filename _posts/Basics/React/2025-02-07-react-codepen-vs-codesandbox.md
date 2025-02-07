---
title: "CodePen vs CodeSandbox: React.js 개발에 어떤 것이 더 좋을까?"
excerpt: "React.js로 간단한 To-Do List 같은 앱을 만들 때 CodePen과 CodeSandbox 중 어떤 것을 사용해야 할까? 각 플랫폼의 장단점을 비교하고, React 앱을 실행하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - React
  - Frontend
  - 개발 도구
tags:
  - [React, JavaScript, Frontend, CodePen, CodeSandbox]
permalink: /react/codepen-vs-codesandbox/
toc: true
toc_sticky: true
date: 2025-02-07
last_modified_at: 2025-02-07
---

React.js로 간단한 **To-Do List** 같은 앱을 만들 때 **CodePen**과 **CodeSandbox** 중 어떤 것이 더 적합할까? 두 가지 플랫폼의 차이점과 React 앱을 실행하는 방법을 자세히 알아보자.

## ✅ CodePen에서 React.js 앱 실행하는 방법

**CodePen**은 HTML, CSS, JavaScript를 한 파일에서 다룰 수 있는 프론트엔드 개발용 온라인 코드 에디터다. 그러나 기본적으로 React 환경이 설정되어 있지 않으므로 CDN을 추가해야 한다.

### 🔹 CodePen에서 React 설정 방법
1. [CodePen](https://codepen.io/)에 접속 후 **"Create New Pen"** 클릭
2. **Settings (JS 설정)** → **"Add External Scripts"**에서 아래 CDN 추가

   ```plaintext
   https://unpkg.com/react@18/umd/react.development.js
   https://unpkg.com/react-dom@18/umd/react-dom.development.js
   https://unpkg.com/@babel/standalone/babel.min.js
   ```

3. JavaScript 코드 영역에서 `<script type="text/babel">`을 사용하여 React 코드 작성
4. 자동 실행됨! 🚀

### 🔹 CodePen에서 React To-Do List 예제 코드
```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>React To-Do List</title>
  <script src="https://unpkg.com/react@18/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
</head>
<body>
  <div id="root"></div>
  
  <script type="text/babel">
    function App() {
      const [tasks, setTasks] = React.useState([]);
      const [input, setInput] = React.useState("");

      const addTask = () => {
        if (input.trim()) {
          setTasks([...tasks, input]);
          setInput("");
        }
      };

      return (
        <div>
          <h1>To-Do List</h1>
          <input
            type="text"
            value={input}
            onChange={(e) => setInput(e.target.value)}
          />
          <button onClick={addTask}>추가</button>
          <ul>
            {tasks.map((task, index) => (
              <li key={index}>{task}</li>
            ))}
          </ul>
        </div>
      );
    }
    ReactDOM.createRoot(document.getElementById("root")).render(<App />);
  </script>
</body>
</html>
```

## ✅ CodeSandbox에서 React.js 앱 실행하는 방법

**CodeSandbox**는 React 개발에 최적화된 온라인 코드 에디터다. 기본적으로 React 환경이 설정되어 있어 별도로 CDN을 추가할 필요가 없다.

### 🔹 CodeSandbox에서 To-Do List 앱 실행 방법
1. [CodeSandbox](https://codesandbox.io/)에 접속
2. **"Create Sandbox"** → **"React"** 템플릿 선택
3. `App.js` 파일을 열고 To-Do List 코드 작성
4. 변경 사항 저장하면 자동으로 실행됨

### 🔹 CodeSandbox에서 React To-Do List 예제 코드
```javascript
import { useState } from "react";

export default function App() {
  const [tasks, setTasks] = useState([]);
  const [input, setInput] = useState("");

  const addTask = () => {
    if (input.trim()) {
      setTasks([...tasks, input]);
      setInput("");
    }
  };

  return (
    <div>
      <h1>To-Do List</h1>
      <input
        type="text"
        value={input}
        onChange={(e) => setInput(e.target.value)}
      />
      <button onClick={addTask}>추가</button>
      <ul>
        {tasks.map((task, index) => (
          <li key={index}>{task}</li>
        ))}
      </ul>
    </div>
  );
}
```

## 🆚 CodePen vs CodeSandbox 비교
|  | **CodePen** | **CodeSandbox** |
|---|---|---|
| **설치 필요 없음** | ✅ | ✅ |
| **React 기본 지원** | ❌ (CDN 추가 필요) | ✅ (바로 사용 가능) |
| **파일 구조 지원** | ❌ (HTML, CSS, JS 한 파일) | ✅ (폴더, 여러 파일 가능) |
| **백엔드 연동 가능** | ❌ (프론트엔드 전용) | ✅ (Express 등 서버 코드도 가능) |
| **빠른 테스트용** | ✅ | ✅ |
| **실제 프로젝트 개발** | ❌ | ✅ |

## 🎯 결론: 언제 어떤 것을 사용해야 할까?
- **간단한 UI 테스트, 작은 컴포넌트 제작** → CodePen 사용
- **실제 React 프로젝트 개발 (폴더 구조 필요)** → CodeSandbox 사용 ✅
- **MERN 스택 같은 백엔드 포함 프로젝트** → CodeSandbox 또는 Vercel, Netlify 활용

### 📢 마무리
CodePen과 CodeSandbox는 각각의 용도에 맞게 활용하면 개발 속도를 높이는 데 큰 도움이 된다. React를 본격적으로 배우고 있다면 **CodeSandbox를 추천**하며, 간단한 UI 실험이 필요하다면 CodePen도 유용하다! 🚀

