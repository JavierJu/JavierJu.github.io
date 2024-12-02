---
title: "Ping Pong 점수 기록기: JavaScript와 Bulma로 배우는 실용적인 예제"
excerpt: "Ping Pong 점수 기록기를 통해 JavaScript의 주요 개념과 Bulma CSS를 활용한 UI 제작을 학습합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, HTML, CSS, Bulma, DOM Manipulation]
permalink: /JavaScript/ping-pong-score-keeper/
toc: true
toc_sticky: true
date: 2024-12-2
last_modified_at: 2024-12-2
---

Ping Pong 점수 기록기는 두 플레이어의 점수를 기록하고 특정 목표 점수에 도달하면 게임을 종료하거나 초기화할 수 있는 간단한 웹 애플리케이션입니다.

## 예제 개요

- **사용 기술**: HTML, CSS(Bulma), JavaScript
- **주요 개념**: DOM 조작, 이벤트 핸들링, 함수 활용, 객체 지향 프로그래밍

---

## 완성 코드

### HTML 코드 (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Score Keeper</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.0/css/bulma.min.css">
</head>

<body>
    <section class="section">
        <div class="container">
            <div class="columns">
                <div class="column is-half is-offset-one-quarter">
                    <div class="card">
                        <div class="card-image">
                            <figure class="image is-2by1">
                                <img src="https://images.unsplash.com/photo-1534158914592-062992fbe900?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3784&q=80"
                                    alt="">
                            </figure>
                        </div>
                        <header class="card-header">
                            <p class="card-header-title">
                                Ping Pong Score Keeper
                            </p>
                        </header>
                        <div class="card-content">
                            <div class="content">
                                <h1 class="title is-1"><span id="p1Display">0</span> to <span id="p2Display">0</span>
                                </h1>
                                <p class="subtitle">Use the buttons below to keep score</p>

                                <label for="playto" class="label is-large is-inline">Playing To</label>
                                <div class="select is-rounded">
                                    <select name="" id="playto">
                                        <option value="3">3</option>
                                        <option value="4">4</option>
                                        <option value="5">5</option>
                                        <option value="6">6</option>
                                        <option value="7">7</option>
                                        <option value="8">8</option>
                                        <option value="9">9</option>
                                        <option value="10">10</option>
                                        <option value="11">11</option>
                                    </select>
                                </div>
                            </div>
                        </div>
                        <footer class="card-footer">
                            <button id="p1Button" class="is-primary button card-footer-item is-large">+1 Player
                                One</button>
                            <button id="p2Button" class="is-info button card-footer-item is-large">+1 Player
                                Two</button>
                            <button id="reset" class="is-danger button card-footer-item is-large">Reset</button>
                        </footer>
                    </div>
                </div>
            </div>
        </div>
    </section>
    <script src="app.js"></script>
</body>

</html>
```

### JavaScript 코드 (`app.js`)

```js
const p1 = {
    score: 0,
    button: document.querySelector('#p1Button'),
    display: document.querySelector('#p1Display')
}
const p2 = {
    score: 0,
    button: document.querySelector('#p2Button'),
    display: document.querySelector('#p2Display')
}

const resetButton = document.querySelector('#reset');
const winningScoreSelect = document.querySelector('#playto');
let winningScore = 3;
let isGameOver = false;

function updateScores(player, opponent) {
    if (!isGameOver) {
        player.score += 1;
        if (player.score === winningScore) {
            isGameOver = true;
            player.display.classList.add('has-text-success');
            opponent.display.classList.add('has-text-danger');
            player.button.disabled = true;
            opponent.button.disabled = true;
        }
        player.display.textContent = player.score;
    }
}

p1.button.addEventListener('click', function () {
    updateScores(p1, p2)
})
p2.button.addEventListener('click', function () {
    updateScores(p2, p1)
})

winningScoreSelect.addEventListener('change', function () {
    winningScore = parseInt(this.value);
    reset();
})

resetButton.addEventListener('click', reset)

function reset() {
    isGameOver = false;
    for (let p of [p1, p2]) {
        p.score = 0;
        p.display.textContent = 0;
        p.display.classList.remove('has-text-success', 'has-text-danger');
        p.button.disabled = false;
    }
}
```

---

## 주요 코드 분석

1. **객체 사용**:
   - 각 플레이어(`p1`, `p2`)를 객체로 정의하여 관련 데이터(`score`)와 DOM 요소를 그룹화.
   - 반복 코드를 줄이고 구조적 관리가 가능.

2. **이벤트 핸들링**:
   - 버튼 클릭 이벤트를 통해 점수를 증가시키고, 게임 종료 또는 초기화 이벤트를 처리.

3. **DOM 조작**:
   - `textContent`, `classList` 등을 활용해 화면에 실시간으로 점수와 스타일을 반영.

4. **Bulma 활용**:
   - CSS 클래스만으로 간결하게 스타일링을 적용하여 UI를 직관적이고 세련되게 표현.

---

## 활용 포인트

이 예제는 JavaScript의 기초 개념을 익히기에 매우 적합합니다. 객체, DOM 조작, 이벤트 핸들링 등을 직접 사용하면서 실제로 작동하는 애플리케이션을 구현할 수 있습니다.
