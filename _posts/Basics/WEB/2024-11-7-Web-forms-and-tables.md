---
title: "HTML 폼과 테이블: 기본 요소와 활용 방법"
excerpt: "HTML에서 폼과 테이블을 사용하여 데이터를 입력하고 표시하는 방법을 배우고, 주요 요소와 속성을 정리합니다."
categories:
  - Web
tags:
  - [HTML, Web Development, Forms, Tables, Accessibility]
permalink: /Web/forms-and-tables/
toc: true
toc_sticky: true
date: 2024-11-7
last_modified_at: 2024-11-7
---

HTML에서 폼과 테이블은 사용자 입력과 데이터를 구조적으로 표시하는 데 필수적인 요소입니다. 이 글에서는 폼과 테이블의 기본 요소와 주요 속성을 정리하고 활용 방법을 소개합니다.

## 폼(Form)

폼은 사용자가 데이터를 입력하거나 서버에 요청을 보낼 수 있도록 설계된 HTML 요소입니다. 다음은 폼과 관련된 주요 요소들입니다.

### 기본 구조
```html
<form action="/submit" method="POST">
  <!-- 폼의 요소들이 여기에 위치 -->
</form>
```

- **`action`**: 데이터를 전송할 URL을 지정합니다.
- **`method`**: 데이터 전송 방식 (`GET` 또는 `POST`)을 지정합니다.

---

### 주요 폼 요소

#### 레이블 (`<label>`)
폼 요소에 대한 설명을 제공하며, 접근성을 향상시킵니다.
```html
<label for="username">사용자 이름:</label>
<input type="text" id="username" name="username">
```
- **`for` 속성**: 입력 필드의 `id`와 연결됩니다. 레이블을 클릭하면 해당 입력 필드가 활성화됩니다.

---

#### 입력 필드 (`<input>`)
사용자 입력을 받을 수 있는 다양한 유형의 필드입니다.

| **타입**       | **설명**                                                                                   | **예제 코드**                                                                                     |
|-----------------|-------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| `text`         | 기본 텍스트 입력 필드                                                                      | `<input type="text" name="name">`                                                                |
| `password`     | 비밀번호 입력 (텍스트가 숨겨짐)                                                           | `<input type="password" name="password">`                                                       |
| `radio`        | 라디오 버튼 (선택지 중 하나만 선택 가능)                                                  | `<input type="radio" name="gender" value="male"> 남성 <input type="radio" name="gender" value="female"> 여성` |
| `checkbox`     | 체크박스 (여러 항목 선택 가능)                                                            | `<input type="checkbox" name="agree" value="yes"> 동의함`                                       |
| `number`       | 숫자 입력                                                                                 | `<input type="number" name="age" min="0" max="120">`                                            |
| `range`        | 슬라이더로 범위 선택                                                                      | `<input type="range" name="volume" min="0" max="100">`                                          |
| `email`        | 이메일 주소 입력 (형식 검증 포함)                                                         | `<input type="email" name="email">`                                                             |
| `file`         | 파일 업로드                                                                               | `<input type="file" name="fileUpload">`                                                         |
| `submit`       | 폼 제출 버튼                                                                              | `<input type="submit" value="제출">`                                                            |
| `button`       | 일반 버튼 (기본 동작 없음, JavaScript와 함께 사용)                                        | `<input type="button" value="클릭">`                                                            |

---

#### 선택창 (`<select>`와 `<option>`)
드롭다운 메뉴로 여러 항목 중 하나를 선택할 수 있습니다.
```html
<select name="country">
  <option value="KR">한국</option>
  <option value="US">미국</option>
  <option value="JP">일본</option>
</select>
```

---

#### 텍스트 영역 (`<textarea>`)
여러 줄 입력이 가능한 텍스트 입력 필드입니다.
```html
<textarea name="message" rows="4" cols="50">여기에 메시지를 입력하세요...</textarea>
```

---

#### 버튼 (`<button>`)
사용자 동작을 트리거할 수 있는 버튼입니다.
```html
<button type="submit">제출</button>
<button type="button" onclick="alert('클릭됨')">클릭</button>
```

---

## 테이블(Table)

테이블은 데이터를 행과 열로 정리하여 표시하는 데 사용됩니다.

### 기본 구조
```html
<table border="1">
  <thead>
    <tr>
      <th>이름</th>
      <th>나이</th>
      <th>직업</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>홍길동</td>
      <td>30</td>
      <td>개발자</td>
    </tr>
    <tr>
      <td>김영희</td>
      <td>25</td>
      <td>디자이너</td>
    </tr>
  </tbody>
</table>
```
- **`<thead>`**: 테이블의 헤더 부분.
- **`<tbody>`**: 테이블 본문.
- **`<tr>`**: 테이블 행.
- **`<th>`**: 헤더 셀.
- **`<td>`**: 데이터 셀.

---

### 병합 기능: `colspan`과 `rowspan`
셀을 가로 또는 세로로 병합할 수 있습니다.
```html
<table border="1">
  <tr>
    <td rowspan="2">병합된 셀 (세로)</td>
    <td>셀 1</td>
    <td>셀 2</td>
  </tr>
  <tr>
    <td colspan="2">병합된 셀 (가로)</td>
  </tr>
</table>
```

---

### CSS를 활용한 스타일링
```html
<style>
  table {
    width: 100%;
    border-collapse: collapse;
  }
  th, td {
    border: 1px solid black;
    padding: 8px;
    text-align: left;
  }
  th {
    background-color: #f2f2f2;
  }
</style>
```

---

## 폼과 테이블의 통합 예제
폼과 테이블을 함께 활용하여 데이터를 입력받는 인터페이스를 구성할 수 있습니다.
```html
<form action="/submit" method="POST">
  <table border="1">
    <tr>
      <th>항목</th>
      <th>입력</th>
    </tr>
    <tr>
      <td>이름</td>
      <td><input type="text" name="name"></td>
    </tr>
    <tr>
      <td>나이</td>
      <td><input type="number" name="age"></td>
    </tr>
    <tr>
      <td>성별</td>
      <td>
        <input type="radio" name="gender" value="male"> 남성
        <input type="radio" name="gender" value="female"> 여성
      </td>
    </tr>
    <tr>
      <td>국가</td>
      <td>
        <select name="country">
          <option value="KR">한국</option>
          <option value="US">미국</option>
          <option value="JP">일본</option>
        </select>
      </td>
    </tr>
  </table>
  <button type="submit">제출</button>
</form>
```

위 내용을 활용하여 폼과 테이블을 조합한 사용자 친화적인 웹 인터페이스를 구축해 보세요!
