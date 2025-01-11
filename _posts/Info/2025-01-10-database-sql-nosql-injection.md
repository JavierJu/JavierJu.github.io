---
title: "SQL Injection과 NoSQL Injection의 이해와 방어 기법"
excerpt: "SQL Injection과 NoSQL Injection의 정의, 공격 기법, 사례, 그리고 효과적인 방어 전략을 상세히 설명합니다."
categories:
  - Database
  - Security
tags:
  - [SQL, NoSQL, Injection, Database Security, MongoDB]
permalink: /info/database-sql-nosql-injection/
toc: true
toc_sticky: true
date: 2025-01-10
last_modified_at: 2025-01-10
---

## SQL Injection

### 정의
SQL Injection은 공격자가 응용 프로그램의 데이터베이스와 상호 작용하는 SQL 쿼리에 악의적인 코드를 삽입하는 공격 기법입니다. 이를 통해 공격자는 인증을 우회하거나, 데이터베이스에서 민감한 데이터를 읽고 수정하거나, 심지어 데이터를 삭제할 수도 있습니다.

---

### 공격의 원리
SQL Injection은 주로 사용자가 입력한 값이 SQL 쿼리에 안전하게 처리되지 않았을 때 발생합니다. 예를 들어, 다음과 같은 쿼리를 생각해 봅시다:

```sql
SELECT * FROM users WHERE username = 'input' AND password = 'input';
```

위와 같은 SQL 쿼리를 처리하는 애플리케이션에서, 사용자가 `username`에 `admin' --` 같은 값을 입력하면 쿼리는 다음과 같이 변합니다:

```sql
SELECT * FROM users WHERE username = 'admin' --' AND password = 'input';
```

`--`는 SQL에서 주석을 의미하므로, 이후의 코드는 무시됩니다. 결과적으로 쿼리는 다음과 같이 실행됩니다:

```sql
SELECT * FROM users WHERE username = 'admin';
```

이로 인해 공격자는 비밀번호 없이도 관리자 계정으로 인증을 우회할 수 있습니다.

---

### SQL Injection의 종류
1. **Error-based SQL Injection**
   - 데이터베이스 오류 메시지를 기반으로 정보를 추출하는 방법입니다.
   - 예: `UNION SELECT`를 사용해 데이터베이스 정보를 가져옴.

2. **Blind SQL Injection**
   - 데이터베이스의 오류 메시지가 숨겨져 있는 경우 사용됩니다.
   - 참/거짓 조건을 반복적으로 테스트하며 데이터를 추출합니다.
   - 예: `if (1=1)`와 같은 조건을 통해 반응을 분석.

3. **Time-based SQL Injection**
   - 특정 쿼리 실행 후 지연 시간을 추가해 결과를 추측합니다.
   - 예: `SELECT IF(1=1, SLEEP(5), 0)`.

4. **Union-based SQL Injection**
   - `UNION` 연산자를 이용해 공격자가 원하는 데이터를 결합하여 반환합니다.
   - 예: `SELECT id, username FROM users UNION SELECT null, version()`.

---

### 방어 방법
1. **Prepared Statements (Parameterized Queries)**
   - SQL 쿼리를 미리 컴파일하고 사용자 입력을 안전하게 처리합니다.
   - 예: 
     ```python
     cursor.execute("SELECT * FROM users WHERE username = ? AND password = ?", (username, password))
     ```

2. **입력 검증**
   - 사용자 입력 값에 허용된 문자 집합과 형식을 정의합니다.
   - 예: 이메일 주소 형식 또는 숫자만 허용.

3. **출력 인코딩**
   - 사용자 입력을 출력하기 전에 적절히 인코딩하여 코드 실행을 방지합니다.

4. **최소 권한 정책**
   - 데이터베이스 사용자 계정에 최소한의 권한만 부여하여 피해를 줄입니다.

5. **WAF (Web Application Firewall)**
   - SQL Injection 공격 패턴을 탐지하고 차단합니다.

---

## NoSQL Injection (MongoDB Injection)

### 정의
NoSQL Injection은 SQL Injection과 유사하게 데이터베이스와의 쿼리에 악의적인 입력을 주입하여 데이터를 무단으로 접근하거나 수정하는 공격입니다. NoSQL 데이터베이스는 SQL과 다른 구조를 사용하지만, 사용자 입력을 신뢰하거나 제대로 처리하지 않을 경우에도 공격에 취약할 수 있습니다.

---

### 공격의 원리
MongoDB의 경우, 쿼리가 JavaScript 객체로 작성됩니다. 예를 들어, 아래와 같은 코드가 있다고 가정합니다:

```javascript
db.users.find({ username: input.username, password: input.password });
```

사용자가 `username`에 다음과 같은 값을 입력한다고 가정합니다:
```json
{ "$ne": null }
```

위의 값이 쿼리에 삽입되면, 실제 실행되는 쿼리는 다음과 같이 변할 수 있습니다:

```javascript
db.users.find({ username: { "$ne": null }, password: input.password });
```

이 경우 `username`이 `null`이 아닌 모든 문서를 반환하므로, 인증이 우회됩니다.

---

### NoSQL Injection의 종류
1. **Boolean-based Injection**
   - 참/거짓 조건을 사용하여 데이터를 추측합니다.
   - 예: `{ "$or": [ { "username": "admin" }, { "password": { "$ne": "" } } ] }`

2. **JavaScript Injection**
   - MongoDB에서 `eval()` 같은 JavaScript 실행 기능을 악용합니다.
   - 예: `db.users.find({ "$where": "this.username == 'admin' && this.password == 'pass'" });`

3. **Union Injection**
   - 데이터베이스의 스키마나 정보를 유추하기 위해 조합된 결과를 요청합니다.

---

### 방어 방법
1. **사용자 입력 검증**
   - 사용자 입력에서 `$`, `.`, `{`, `}` 같은 특수 문자를 필터링합니다.

2. **ORM/ODM 사용**
   - Mongoose 같은 라이브러리를 사용하면 쿼리를 자동으로 파라미터화하고 보안을 강화합니다.

3. **최소 권한 정책**
   - SQL과 동일하게 데이터베이스 사용자에게 최소 권한만 부여합니다.

4. **MongoDB Query Validation**
   - 쿼리를 실행하기 전에 JSON 스키마 검증을 통해 유효성을 확인합니다.

---

### 결론
SQL Injection과 NoSQL Injection은 각각의 데이터베이스 특성에 따라 다르지만, 공통적으로 사용자 입력을 신뢰하지 않고 철저히 검증 및 처리하는 것이 가장 중요한 방어 전략입니다. MongoDB와 같은 NoSQL 데이터베이스에서도 Injection 공격이 발생할 수 있으므로 적절한 방어 기법을 적용해야 합니다.
