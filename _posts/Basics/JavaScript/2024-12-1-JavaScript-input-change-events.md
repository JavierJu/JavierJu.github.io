---
title: "JavaScript DOM 이벤트: 입력과 변경 이벤트"
excerpt: "JavaScript의 DOM 이벤트 중 입력 이벤트(input)와 변경 이벤트(change)에 대해 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, DOM, Event, Input, Change]
permalink: /JavaScript/input-change-events/
toc: true
toc_sticky: true
date: 2024-12-1
last_modified_at: 2024-12-1
---

JavaScript에서 **DOM 이벤트**는 사용자의 상호작용을 감지하고 이에 반응하기 위해 사용됩니다. 이 중 **입력 이벤트(input)**와 **변경 이벤트(change)**는 주로 사용자 입력에 반응하는 데 사용됩니다. 두 이벤트는 비슷한 점도 있지만 발생하는 조건과 사용 목적에 있어 차이가 있습니다. 아래에서 이 두 이벤트에 대해 자세히 살펴보겠습니다.

## 1. 입력 이벤트 (input event)

### 특징
- **입력 이벤트**는 사용자가 입력 필드에 **내용을 입력하거나 수정할 때마다** 발생합니다.
- 주로 `<input>`, `<textarea>`, `<select>` 요소에서 사용됩니다.
- 실시간으로 값 변경을 감지해야 할 때 유용합니다.

### 사용 예시
```js
<input type="text" id="name" placeholder="Enter your name">
<p id="output"></p>

<script>
  const inputField = document.getElementById("name");
  const output = document.getElementById("output");

  inputField.addEventListener("input", (event) => {
    output.textContent = `입력된 값: ${event.target.value}`;
  });
</script>
```

### 발생 조건
- 키보드로 입력했을 때
- 값을 붙여넣었을 때
- 값을 삭제했을 때

### 주요 사용 시나리오
- 실시간 검색 기능
- 양식 입력 유효성 검사
- 입력값을 실시간으로 표시하는 데이터 시각화

## 2. 변경 이벤트 (change event)

### 특징
- **변경 이벤트**는 사용자가 입력 필드의 값을 변경하고, **입력 요소에서 포커스를 잃을 때(blur)** 발생합니다.
- 주로 `<input>`, `<textarea>`, `<select>` 요소에서 사용됩니다.
- 값 변경이 완료된 후 발생하므로, 입력 중 실시간으로 반응할 필요가 없는 경우에 적합합니다.

### 사용 예시
```js
<select id="fruit">
  <option value="apple">Apple</option>
  <option value="banana">Banana</option>
  <option value="cherry">Cherry</option>
</select>
<p id="output"></p>

<script>
  const selectField = document.getElementById("fruit");
  const output = document.getElementById("output");

  selectField.addEventListener("change", (event) => {
    output.textContent = `선택된 과일: ${event.target.value}`;
  });
</script>
```

### 발생 조건
- 사용자가 선택한 값이 변경된 후 포커스를 다른 요소로 이동할 때

### 주요 사용 시나리오
- 드롭다운 메뉴 선택 시 반응
- 양식 제출 전 최종 값 확인
- 체크박스나 라디오 버튼 상태 변경 시

---

이와 같이, **입력 이벤트(input)**는 값이 실시간으로 변할 때마다 반응하고, **변경 이벤트(change)**는 값이 변경되고 나서 포커스를 잃을 때 발생합니다. 두 이벤트는 상황에 맞게 적절히 사용하면 사용자 경험을 크게 향상시킬 수 있습니다.
