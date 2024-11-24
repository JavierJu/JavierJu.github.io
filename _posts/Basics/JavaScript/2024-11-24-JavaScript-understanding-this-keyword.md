---
title: "JavaScript 변수명 규칙: 카멜 표기법부터 상수 표기법까지"
excerpt: "JavaScript에서 변수명을 작성할 때 사용하는 카멜 표기법(Camel Case)과 다른 주요 표기법들에 대해 알아보고, 각 표기법의 사용 사례와 장점을 살펴봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Coding Standards, Best Practices, Variables, Naming Conventions]
permalink: /JavaScript/variable-naming-conventions/
toc: true
toc_sticky: true
date: 2024-11-23
last_modified_at: 2024-11-23
---

### JavaScript 변수명 규칙: 왜 카멜 표기법을 사용할까?

JavaScript를 포함한 대부분의 프로그래밍 언어에서 변수와 함수 이름은 일정한 표기법을 따릅니다. 특히, **카멜 표기법(Camel Case)**은 JavaScript에서 가장 일반적으로 사용되는 방식입니다. 이 글에서는 카멜 표기법과 함께 다른 주요 표기법들, 그리고 코딩 시 알아두면 좋은 명명 규칙을 정리해 보겠습니다.

---

## 1. 카멜 표기법(Camel Case)

**카멜 표기법**은 변수 이름의 첫 단어를 소문자로 시작하고, 두 번째 단어부터는 각 단어의 첫 글자를 대문자로 작성하는 방식입니다.

- 예: `myName`, `yourAddress`, `getUserData`

### **왜 카멜 표기법을 사용할까?**
1. **가독성**: 단어의 경계를 쉽게 구분할 수 있어 코드가 더 읽기 쉽습니다.
2. **JavaScript의 관례**: JavaScript에서는 변수와 함수 이름에 카멜 표기법을 사용하는 것이 표준으로 자리 잡았습니다.
3. **일관성 유지**: 팀 작업 시, 같은 규칙을 따름으로써 코드의 일관성을 유지합니다.

---

## 2. 다른 변수명 표기법

### **(1) 스네이크 표기법(Snake Case)**
단어를 `_`로 구분하고 모든 단어를 소문자로 작성하는 방식입니다.

- 예: `my_name`, `your_address`

**사용 사례**:
- 데이터베이스 필드 이름
- Python과 같은 언어에서 선호

---

### **(2) 파스칼 표기법(Pascal Case)**
모든 단어의 첫 글자를 대문자로 시작하는 방식입니다.

- 예: `MyName`, `YourAddress`, `GetUserData`

**사용 사례**:
- 클래스 이름 (`User`, `ProductManager`)
- 생성자 함수 이름

---

### **(3) 상수 표기법(CONSTANT_CASE)**
모든 글자를 대문자로 작성하고 단어를 `_`로 구분합니다.

- 예: `MAX_RETRY`, `API_KEY`, `DEFAULT_TIMEOUT`

**사용 사례**:
- `const` 키워드로 선언한 상수 값
- 읽기 전용 값이나 환경 변수를 나타낼 때 사용

---

## 3. JavaScript에서의 변수명 규칙 정리

| **사용 대상**       | **표기법**       | **예시**                  |
|---------------------|------------------|---------------------------|
| 변수 및 함수 이름   | Camel Case       | `getUserName`, `isLoggedIn` |
| 클래스 및 생성자 함수 | Pascal Case      | `User`, `ProductManager`  |
| 상수               | CONSTANT_CASE    | `MAX_RETRY`, `API_URL`    |
| 파일 이름          | Snake Case / Kebab Case | `user_data.js`, `user-data.js` |

---

## 4. 언어별 관습 차이
JavaScript뿐만 아니라 다른 프로그래밍 언어에서도 변수명 표기법은 언어의 관례와 목적에 따라 다릅니다.

- **Python**: 변수와 함수는 snake_case, 클래스는 PascalCase.
- **Java**: CamelCase를 변수와 메서드에, PascalCase는 클래스에 사용.
- **C/C++**: 상수는 UPPER_CASE, 나머지는 snake_case나 camelCase 혼용.

---

## 5. 마무리

코드에서 변수명 규칙은 단순히 스타일 문제가 아니라 **코드 가독성과 유지보수성을 높이는 중요한 요소**입니다. JavaScript에서는 주로 **카멜 표기법**을 따르며, 상수는 **CONSTANT_CASE**로 작성하는 것이 관례입니다. 이러한 규칙을 따르면 협업 시 코드의 일관성을 유지할 수 있고, 다른 개발자가 코드를 이해하기 쉬워집니다.

당신의 코드에서도 이러한 규칙을 활용해 보세요!
