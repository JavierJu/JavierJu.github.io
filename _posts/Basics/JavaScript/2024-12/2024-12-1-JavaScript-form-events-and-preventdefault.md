---
title: "JavaScript 폼 이벤트와 preventDefault의 이해와 활용"
excerpt: "JavaScript에서 폼 이벤트와 preventDefault를 활용하는 방법을 다양한 예제와 함께 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Form, Event, preventDefault, Web Development]
permalink: /JavaScript/form-events-and-preventdefault/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

JavaScript에서 폼 이벤트와 `preventDefault`는 웹 폼의 동작을 제어하거나 사용자 경험을 개선하는 데 중요한 역할을 합니다. 이번 글에서는 이 두 가지를 다루며 다양한 예제를 통해 구체적으로 이해해 보겠습니다.

---

## **1. 폼 이벤트란?**
폼 이벤트는 HTML 폼과 상호작용할 때 발생하는 다양한 이벤트를 말합니다. 대표적인 폼 이벤트는 다음과 같습니다:

- `submit`: 폼이 제출될 때 발생.
- `input`: 사용자가 입력 필드에 값을 입력할 때마다 발생.
- `change`: 사용자가 입력 필드의 값을 변경하고 초점을 다른 곳으로 옮길 때 발생.
- `focus`: 사용자가 입력 필드를 선택할 때 발생.
- `blur`: 입력 필드에서 초점이 벗어날 때 발생.

---

## **2. preventDefault란?**
`preventDefault()`는 브라우저의 기본 동작을 막는 메서드입니다. 예를 들어, 폼 제출 시 페이지가 새로고침되는 기본 동작을 방지하고, JavaScript로 직접 동작을 제어할 수 있습니다.

### **사용 예시**
```javascript
formElement.addEventListener('submit', function (event) {
  event.preventDefault(); // 기본 동작 방지
  console.log('폼이 제출되었지만 새로고침은 발생하지 않았습니다.');
});
```

---

## **3. 폼 이벤트와 preventDefault 활용 예제**

### **3.1 기본 폼 제출 동작 제어**
사용자가 제출한 폼 데이터를 확인한 후, 새로고침 없이 결과를 표시하는 예제입니다.

HTML 코드
```html
<form id="basicForm">
  <label for="username">Username:</label>
  <input type="text" id="username" name="username" required />
  <label for="password">Password:</label>
  <input type="password" id="password" name="password" required />
  <button type="submit">Submit</button>
</form>
<div id="output"></div>
```
JavaScript 코드
```javascript
document.getElementById('basicForm').addEventListener('submit', function (event) {
  event.preventDefault();

  const formData = new FormData(event.target);
  const username = formData.get('username');
  const password = formData.get('password');

  const outputDiv = document.getElementById('output');
  if (!username || !password) {
    outputDiv.textContent = 'Please fill out all fields.';
    outputDiv.style.color = 'red';
  } else {
    outputDiv.textContent = `Submitted! Username: ${username}, Password: ${'*'.repeat(password.length)}`;
    outputDiv.style.color = 'green';
  }
});
```

---

### **3.2 실시간 입력 검증**
입력 필드의 값을 실시간으로 검증하여 유효하지 않을 경우 경고 메시지를 표시합니다.

HTML 코드
```html
<form>
  <label for="email">Email:</label>
  <input type="email" id="email" name="email" placeholder="Enter your email" required />
  <small id="emailHint" style="color: red; display: none;">Please enter a valid email address.</small>
</form>
```
JavaScript 코드
```javascript
const emailInput = document.getElementById('email');
const emailHint = document.getElementById('emailHint');

emailInput.addEventListener('input', function () {
  const emailValue = emailInput.value;

  if (!emailValue.includes('@') || !emailValue.includes('.')) {
    emailHint.style.display = 'inline';
  } else {
    emailHint.style.display = 'none';
  }
});
```

---

### **3.3 AJAX를 사용한 폼 제출**
페이지 새로고침 없이 폼 데이터를 서버로 전송하고, 서버 응답을 처리하는 예제입니다.

HTML 코드
```html
<form id="ajaxForm">
  <label for="name">Name:</label>
  <input type="text" id="name" name="name" required />
  <button type="submit">Send</button>
</form>
<div id="response"></div>
```
JavaScript 코드
```javascript
document.getElementById('ajaxForm').addEventListener('submit', async function (event) {
  event.preventDefault();

  const formData = new FormData(event.target);
  const responseDiv = document.getElementById('response');

  try {
    const response = await fetch('/submit', {
      method: 'POST',
      body: formData,
    });

    if (response.ok) {
      const result = await response.json();
      responseDiv.textContent = `Success: ${result.message}`;
      responseDiv.style.color = 'green';
    } else {
      throw new Error('Request failed!');
    }
  } catch (error) {
    responseDiv.textContent = `Error: ${error.message}`;
    responseDiv.style.color = 'red';
  }
});
```

---

### **3.4 다단계 폼 제어**
여러 단계를 거치는 폼을 구현할 때 특정 단계로 이동하거나 검증하는 방법입니다.

HTML 코드
```html
<form id="multiStepForm">
  <div id="step1" class="step">
    <label for="step1Name">Step 1: Name</label>
    <input type="text" id="step1Name" name="name" required />
    <button type="button" id="nextToStep2">Next</button>
  </div>

  <div id="step2" class="step" style="display: none;">
    <label for="step2Email">Step 2: Email</label>
    <input type="email" id="step2Email" name="email" required />
    <button type="submit">Submit</button>
  </div>
</form>
<div id="multiStepOutput"></div>
```
JavaScript 코드
```javascript
const step1 = document.getElementById('step1');
const step2 = document.getElementById('step2');
const nextButton = document.getElementById('nextToStep2');
const form = document.getElementById('multiStepForm');
const output = document.getElementById('multiStepOutput');

nextButton.addEventListener('click', function () {
  const nameInput = document.getElementById('step1Name').value;

  if (!nameInput.trim()) {
    alert('Please enter your name!');
  } else {
    step1.style.display = 'none';
    step2.style.display = 'block';
  }
});

form.addEventListener('submit', function (event) {
  event.preventDefault();

  const formData = new FormData(form);
  const name = formData.get('name');
  const email = formData.get('email');

  output.textContent = `Form submitted! Name: ${name}, Email: ${email}`;
  output.style.color = 'green';
});
```

---

## **결론**
JavaScript에서 폼 이벤트와 `preventDefault`는 기본 동작을 제어하고 사용자 경험을 향상시키는 데 필수적인 도구입니다. 다양한 폼 관련 작업에서 이를 적절히 활용하면 더 나은 웹 애플리케이션을 구현할 수 있습니다.
