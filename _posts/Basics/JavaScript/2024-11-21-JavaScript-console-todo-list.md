---
title: "JavaScript로 만드는 간단한 To-Do List 프로젝트"
excerpt: "JavaScript로 간단한 콘솔 기반 To-Do List를 만드는 방법과 코드 구조를 자세히 설명합니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Projects, Tutorials, Beginner]
permalink: /JavaScript/console-todo-list/
toc: true
toc_sticky: true
date: 2024-11-21
last_modified_at: 2024-11-21
---

## 소개

이 글에서는 JavaScript를 사용하여 간단한 콘솔 기반 To-Do List 프로그램을 만드는 방법을 소개합니다. 이 프로그램은 사용자가 할 일을 추가, 삭제하거나 목록을 확인할 수 있는 기능을 제공합니다. 코드를 단계별로 분석하며 구조를 이해하고 확장할 수 있는 방법도 알아봅니다.

---

## 코드 전체보기

To-Do List 프로그램의 전체 코드는 다음과 같습니다:

```js
let input = prompt('what would you like to do?');
const todos = ['Collect Chicken Eggs', 'Clean Litter Box'];
while (input !== 'quit' && input !== 'q') {
    if (input === 'list') {
        console.log('*****************');
        for (let i = 0; i < todos.length; i++) {
            console.log(`${i}: ${todos[i]}`);
        }
        console.log('*****************');
    } else if (input === 'new') {
        const newTodo = prompt('Ok, what is the new todo?');
        todos.push(newTodo);
        console.log(`${newTodo} added to the list!`);
    } else if (input === 'delete') {
        const index = parseInt(prompt('Ok, enter an index to delete:'));
        if (!Number.isNaN(index)) {
            const deleted = todos.splice(index, 1);
            console.log(`Ok, deleted ${deleted[0]}`);
        } else {
            console.log('Unknown index');
        }
    }
    input = prompt('what would you like to do?');
}
console.log('OK QUIT THE APP!');
```

---

## 코드 분석

### 1. **사용자 입력 받기**

```js
let input = prompt('what would you like to do?');
```

- `prompt`를 사용하여 사용자 입력을 받습니다.
- `input` 변수는 이후 반복문에서 명령어를 처리하는 데 사용됩니다.

---

### 2. **To-Do List 배열 초기화**

```js
const todos = ['Collect Chicken Eggs', 'Clean Litter Box'];
```

- `todos` 배열은 할 일을 저장합니다.
- 초기값으로 두 개의 할 일이 포함되어 있습니다.

---

### 3. **메인 반복문**

```js
while (input !== 'quit' && input !== 'q') {
```

- 이 반복문은 사용자가 `'quit'` 또는 `'q'`를 입력하지 않는 한 계속 실행됩니다.
- 내부에서 입력된 명령어(`input`)에 따라 작업이 수행됩니다.

---

### 4. **명령어 처리**

#### (1) `list` 명령: To-Do 목록 출력

```js
if (input === 'list') {
    console.log('*****************');
    for (let i = 0; i < todos.length; i++) {
        console.log(`${i}: ${todos[i]}`);
    }
    console.log('*****************');
}
```

- `list` 명령은 할 일을 출력합니다.
- 각 항목의 인덱스와 내용을 함께 표시합니다.

#### (2) `new` 명령: 새 할 일 추가

```js
} else if (input === 'new') {
    const newTodo = prompt('Ok, what is the new todo?');
    todos.push(newTodo);
    console.log(`${newTodo} added to the list!`);
}
```

- 사용자로부터 새 할 일을 입력받아 `todos` 배열에 추가합니다.

#### (3) `delete` 명령: 특정 항목 삭제

```js
} else if (input === 'delete') {
    const index = parseInt(prompt('Ok, enter an index to delete:'));
    if (!Number.isNaN(index)) {
        const deleted = todos.splice(index, 1);
        console.log(`Ok, deleted ${deleted[0]}`);
    } else {
        console.log('Unknown index');
    }
}
```

- 삭제할 항목의 **인덱스**를 입력받아 배열에서 제거합니다.
- 숫자가 아닌 경우 에러 메시지를 출력합니다.

---

### 5. **프로그램 종료**

```js
console.log('OK QUIT THE APP!');
```

- 사용자가 `'quit'` 또는 `'q'`를 입력하면 프로그램이 종료되고 메시지를 출력합니다.

---

## 확장 아이디어

1. **HTML/CSS 기반 UI 추가**
   - 이 코드를 브라우저에서 동작하는 UI 기반 To-Do List로 확장할 수 있습니다.
2. **데이터 저장 기능**
   - 로컬 스토리지(localStorage)를 활용해 데이터를 저장하고, 브라우저를 닫아도 데이터가 유지되도록 구현할 수 있습니다.
3. **추가 명령어**
   - 예를 들어, 특정 항목을 수정하는 `edit` 명령을 추가할 수 있습니다.

---

이 간단한 프로젝트는 JavaScript의 기본적인 배열 조작, 사용자 입력 처리, 반복문 등을 연습하는 데 매우 유용합니다. 초보자에게 적합하며, 이를 기반으로 더욱 발전된 웹 애플리케이션을 만들어 보세요!
