---
title: "React 상태 관리와 조건부 렌더링: 간단한 Score Board 만들기 예제제"
excerpt: "React의 상태 관리와 조건부 렌더링을 활용한 간단한 Score Board 컴포넌트를 만들어보고, 각 코드의 역할과 작동 방식을 자세히 설명합니다."
categories:
  - React
  - Frontend
tags:
  - [React, JavaScript, 상태 관리, 조건부 렌더링, Frontend]
permalink: /react/scoreboard-state-management/
toc: true
toc_sticky: true
date: 2025-01-16
last_modified_at: 2025-01-16
---

## React 상태 관리와 조건부 렌더링: 간단한 Score Board 만들기

React에서 상태 관리와 조건부 렌더링은 핵심적인 개념입니다. 이번 포스트에서는 간단한 Score Board 컴포넌트를 만들어보고, 각 코드의 역할과 작동 방식을 자세히 설명하겠습니다. 또한, 승리 조건을 추가하고 점수 증가를 제한하는 방법도 다룹니다.

### 최종 코드
아래는 최종 코드입니다. 이 코드는 플레이어별 점수를 관리하고, 승자가 결정되면 점수 증가를 멈추는 기능을 포함합니다.

```jsx
import { useState } from "react";

export default function ScoreBoard({ numPlayers = 5, target = 3 }) {
    const [scores, setScores] = useState(new Array(numPlayers).fill(0));
    const [winner, setWinner] = useState(null);

    const increaseScore = (idx) => {
        if (winner !== null) return;

        setScores((oldScores) => {
            const newScores = oldScores.map((score, i) => {
                if (i === idx) return score + 1;
                return score;
            });

            if (newScores[idx] >= target) {
                setWinner(idx);
            }

            return newScores;
        });
    };

    const reset = () => {
        setScores(new Array(numPlayers).fill(0));
        setWinner(null);
    };

    return (
        <>
            <h1>Score Board</h1>
            <ul>
                {scores.map((score, idx) => (
                    <li key={idx}>
                        Player {idx + 1}: {score}
                        <button
                            onClick={() => increaseScore(idx)}
                            disabled={winner !== null}
                        >
                            +1
                        </button>
                        {winner === idx && " \ud83c\udfc6 You Win!"}
                    </li>
                ))}
            </ul>
            <button onClick={reset}>Reset</button>
            {winner !== null && <h2>Player {winner + 1} is the Winner!</h2>}
        </>
    );
}
```

---

### 코드 설명

#### 1. 컴포넌트 정의
```jsx
export default function ScoreBoard({ numPlayers = 5, target = 3 }) {
```
- **`ScoreBoard`**: React 컴포넌트의 이름입니다.
- **`numPlayers`**: 플레이어 수를 나타내는 `props`로 기본값은 `5`입니다.
- **`target`**: 승리 점수로 기본값은 `3`입니다.

#### 2. 상태 관리
```jsx
const [scores, setScores] = useState(new Array(numPlayers).fill(0));
const [winner, setWinner] = useState(null);
```
- **`scores`**: 각 플레이어의 점수를 관리하는 배열 상태입니다.
  - 초기값은 `[0, 0, 0, 0, 0]` (플레이어가 5명일 경우).
- **`winner`**: 승자를 추적하는 상태로, 초기값은 `null`입니다.

#### 3. 점수 증가 함수
```jsx
const increaseScore = (idx) => {
    if (winner !== null) return;

    setScores((oldScores) => {
        const newScores = oldScores.map((score, i) => {
            if (i === idx) return score + 1;
            return score;
        });

        if (newScores[idx] >= target) {
            setWinner(idx);
        }

        return newScores;
    });
};
```
- **`if (winner !== null) return;`**: 이미 승자가 결정된 경우 점수 증가를 멈춥니다.
- **`setScores`**: 상태 업데이트 함수로, 새로운 점수 배열을 생성해 설정합니다.
- **`setWinner(idx)`**: 특정 플레이어가 목표 점수에 도달하면 승자를 설정합니다.

#### 4. 리셋 함수
```jsx
const reset = () => {
    setScores(new Array(numPlayers).fill(0));
    setWinner(null);
};
```
- **`reset`**: 점수와 승자 상태를 초기화하여 게임을 재시작합니다.

#### 5. UI 렌더링
```jsx
<ul>
    {scores.map((score, idx) => (
        <li key={idx}>
            Player {idx + 1}: {score}
            <button
                onClick={() => increaseScore(idx)}
                disabled={winner !== null}
            >
                +1
            </button>
            {winner === idx && " \ud83c\udfc6 You Win!"}
        </li>
    ))}
</ul>
```
- **`scores.map`**: 점수 배열을 반복하면서 각 플레이어의 점수와 버튼을 렌더링합니다.
- **`disabled={winner !== null}`**: 승자가 있을 경우 버튼을 비활성화합니다.
- **`{winner === idx && " \ud83c\udfc6 You Win!"}`**: 승자를 표시합니다.

---

### React에서 배우는 핵심 개념

1. **상태 관리 (`useState`)**:
   - 컴포넌트 내부에서 데이터를 관리하고 업데이트하는 방법.

2. **상태 불변성 유지**:
   - 기존 배열을 수정하지 않고 새로운 배열을 생성하여 상태를 업데이트.

3. **조건부 렌더링**:
   - 특정 조건에 따라 UI를 다르게 렌더링.

4. **이벤트 처리**:
   - 사용자 동작(버튼 클릭)에 따라 상태를 변경하는 방법.

5. **리스트 렌더링**:
   - 배열 데이터를 기반으로 동적 UI를 생성.

---

### 마무리
이 코드 예제를 통해 React의 상태 관리와 조건부 렌더링에 대한 기본적인 이해를 높일 수 있습니다. 간단한 기능부터 점차 복잡한 기능을 추가하며 React를 익혀보세요. 추가적인 질문이나 개선 아이디어가 있다면 댓글로 남겨주세요!

