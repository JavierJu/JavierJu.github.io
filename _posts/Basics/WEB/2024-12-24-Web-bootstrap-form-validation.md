---
title: "Bootstrap 폼 유효성 검사: HTML과 JavaScript를 활용한 사용자 입력 관리"
excerpt: "Bootstrap의 폼 유효성 검사 기능을 이용해 사용자 입력값 검증과 피드백을 제공하는 방법을 자세히 살펴봅니다."
categories:
  - Web
tags:
  - [Bootstrap, HTML, JavaScript, Form Validation]
permalink: /Web/bootstrap-form-validation/
toc: true
toc_sticky: true
date: 2024-12-24
last_modified_at: 2024-12-24
---

## 개요

사용자 입력값을 올바르게 검증하고 오류에 대한 피드백을 제공하는 것은 웹 애플리케이션에서 매우 중요합니다. Bootstrap은 이를 쉽게 구현할 수 있는 폼 유효성 검사 기능을 제공합니다. 이 글에서는 HTML과 JavaScript를 활용하여 Bootstrap 폼 유효성 검사를 구현하는 방법을 예제 코드와 함께 살펴보겠습니다.

---

## HTML 코드 설명

아래는 Bootstrap의 폼 유효성 검사 예제입니다:

```html
<form class="row g-3 needs-validation" novalidate>
  <div class="col-md-4">
    <label for="validationCustom01" class="form-label">First name</label>
    <input type="text" class="form-control" id="validationCustom01" value="Mark" required>
    <div class="valid-feedback">
      Looks good!
    </div>
  </div>
  <div class="col-md-4">
    <label for="validationCustom02" class="form-label">Last name</label>
    <input type="text" class="form-control" id="validationCustom02" value="Otto" required>
    <div class="valid-feedback">
      Looks good!
    </div>
  </div>
  <div class="col-md-4">
    <label for="validationCustomUsername" class="form-label">Username</label>
    <div class="input-group has-validation">
      <span class="input-group-text" id="inputGroupPrepend">@</span>
      <input type="text" class="form-control" id="validationCustomUsername" aria-describedby="inputGroupPrepend" required>
      <div class="invalid-feedback">
        Please choose a username.
      </div>
    </div>
  </div>
  <div class="col-md-6">
    <label for="validationCustom03" class="form-label">City</label>
    <input type="text" class="form-control" id="validationCustom03" required>
    <div class="invalid-feedback">
      Please provide a valid city.
    </div>
  </div>
  <div class="col-md-3">
    <label for="validationCustom04" class="form-label">State</label>
    <select class="form-select" id="validationCustom04" required>
      <option selected disabled value="">Choose...</option>
      <option>...</option>
    </select>
    <div class="invalid-feedback">
      Please select a valid state.
    </div>
  </div>
  <div class="col-md-3">
    <label for="validationCustom05" class="form-label">Zip</label>
    <input type="text" class="form-control" id="validationCustom05" required>
    <div class="invalid-feedback">
      Please provide a valid zip.
    </div>
  </div>
  <div class="col-12">
    <div class="form-check">
      <input class="form-check-input" type="checkbox" value="" id="invalidCheck" required>
      <label class="form-check-label" for="invalidCheck">
        Agree to terms and conditions
      </label>
      <div class="invalid-feedback">
        You must agree before submitting.
      </div>
    </div>
  </div>
  <div class="col-12">
    <button class="btn btn-primary" type="submit">Submit form</button>
  </div>
</form>
```

### 주요 구성 요소

1. **`<form>` 태그**
   - `class="needs-validation"`: Bootstrap의 유효성 검사 스타일을 활성화합니다.
   - `novalidate`: 기본 HTML5 유효성 검사를 비활성화합니다.

2. **입력 필드**
   - 각 입력 필드는 `required` 속성을 사용해 필수 입력값임을 지정합니다.
   - `valid-feedback` 및 `invalid-feedback` 요소를 통해 유효성 메시지를 사용자에게 제공합니다.

3. **체크박스와 버튼**
   - 체크박스는 사용자의 동의를 요구하며, 필수 입력값으로 설정됩니다.
   - 제출 버튼은 폼 제출을 트리거합니다.

---

## JavaScript 코드 설명

다음은 JavaScript로 폼의 유효성을 검사하고 사용자 피드백을 제공하는 코드입니다:

```javascript
(() => {
  'use strict'

  // Fetch all the forms we want to apply custom Bootstrap validation styles to
  const forms = document.querySelectorAll('.needs-validation')

  // Loop over them and prevent submission
  Array.from(forms).forEach(form => {
    form.addEventListener('submit', event => {
      if (!form.checkValidity()) {
        event.preventDefault()
        event.stopPropagation()
      }

      form.classList.add('was-validated')
    }, false)
  })
})()
```

### 작동 원리

1. **폼 선택**
   - `querySelectorAll('.needs-validation')`을 통해 모든 관련 폼을 선택합니다.

2. **이벤트 리스너 등록**
   - 각 폼에 `submit` 이벤트 리스너를 추가하여 제출 시 유효성 검사를 실행합니다.

3. **유효성 검사 처리**
   - `checkValidity()`: 폼 입력값의 유효성을 확인합니다.
   - 유효하지 않을 경우 `event.preventDefault()`로 제출을 차단하고, `was-validated` 클래스를 추가하여 시각적 피드백을 제공합니다.

---

## 결론

Bootstrap의 폼 유효성 검사 기능은 간단한 설정으로 직관적이고 강력한 입력값 검증을 제공합니다. HTML 구조와 JavaScript를 결합하여 사용자 경험을 향상시키고 잘못된 데이터를 방지할 수 있습니다. 위 코드를 활용해 프로젝트에 유효성 검사를 추가해 보세요!

