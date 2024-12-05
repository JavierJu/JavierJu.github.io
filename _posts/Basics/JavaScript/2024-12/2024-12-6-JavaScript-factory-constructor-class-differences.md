---
title: "JavaScript에서 팩토리 함수, 생성자 함수, Class의 차이"
excerpt: "JavaScript에서 객체를 생성하는 다양한 방법인 팩토리 함수, 생성자 함수, Class의 차이점과 사용 사례에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, 객체지향, 함수, Class]
permalink: /JavaScript/factory-constructor-class-differences/
toc: true
toc_sticky: true
date: 2024-12-6
last_modified_at: 2024-12-6
---

JavaScript에서 객체를 생성하는 방법은 크게 **팩토리 함수**, **생성자 함수**, 그리고 **클래스**로 나눌 수 있습니다. 각 방식은 특징과 사용 목적이 다르므로, 상황에 맞게 선택하는 것이 중요합니다. 이번 글에서는 이 세 가지 방법의 차이점과 각각의 장단점을 알아보겠습니다..

### 1. 팩토리 함수 (Factory Function)

팩토리 함수는 객체를 생성하고 반환하는 일반적인 함수입니다. `new` 키워드를 사용하지 않고 객체를 생성할 수 있어, 비교적 간단하고 유연한 방식으로 객체를 만들 수 있습니다.

#### 특징:
- `new` 키워드가 필요하지 않습니다.
- 클로저를 사용하여 private 데이터를 만들 수 있습니다.
- 객체 생성 전에 커스텀 로직을 추가할 수 있습니다.

```javascript
function createPerson(name, age) {
    return {
        name,
        age,
        sayHello() {
            console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
        }
    };
}

const person = createPerson('Alice', 25);
person.sayHello(); // Hi, I'm Alice and I'm 25 years old.
```

#### 장점:
- 객체를 반환하는 로직을 쉽게 커스터마이징 가능.
- `this` 바인딩 문제 없음.

#### 단점:
- 상속 구조 설정이 어려움.
- 메서드를 공유하는 데 번거롭습니다.

---

### 2. 생성자 함수 (Constructor Function)

생성자 함수는 `new` 키워드를 사용하여 객체를 생성하는 함수입니다. 객체는 기본적으로 `prototype`을 통해 메서드를 공유할 수 있습니다.

#### 특징:
- `new` 키워드를 사용해야 합니다.
- `prototype`을 사용하여 메서드를 공유할 수 있습니다.

```javascript
function Person(name, age) {
    this.name = name;
    this.age = age;
}

Person.prototype.sayHello = function() {
    console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
};

const person = new Person('Bob', 30);
person.sayHello(); // Hi, I'm Bob and I'm 30 years old.
```

#### 장점:
- 객체 생성 패턴을 표준화할 수 있음.
- `prototype`을 사용하여 메서드를 공유함으로써 메모리 효율성 증가.

#### 단점:
- `new` 키워드를 사용하지 않으면 의도대로 작동하지 않음.
- private 데이터를 다루는 데 어려움이 있음.

---

### 3. 클래스 (Class)

ES6에서 도입된 `class`는 생성자 함수와 프로토타입 기반 상속을 간결하고 읽기 쉽게 표현한 문법입니다. 객체 지향 프로그래밍 패턴을 따르기 좋습니다.

#### 특징:
- `class` 문법을 사용하여 객체를 생성합니다.
- `constructor`를 통해 초기화하고, `prototype`을 사용한 상속이 기본적으로 구현됩니다.

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    sayHello() {
        console.log(`Hi, I'm ${this.name} and I'm ${this.age} years old.`);
    }
}

const person = new Person('Charlie', 35);
person.sayHello(); // Hi, I'm Charlie and I'm 35 years old.
```

#### 장점:
- 객체지향 프로그래밍 패턴에 적합.
- 상속 구현이 간단하고 직관적.
- 코드가 간결하고 가독성이 높음.

```javascript
class Student extends Person {
    constructor(name, age, grade) {
        super(name, age);
        this.grade = grade;
    }

    introduce() {
        console.log(`Hi, I'm ${this.name}, I'm in grade ${this.grade}.`);
    }
}

const student = new Student('Daisy', 16, 10);
student.introduce(); // Hi, I'm Daisy, I'm in grade 10.
```

#### 단점:
- `class`는 결국 프로토타입 기반으로 동작하므로, JavaScript의 원래 구조를 감추는 추상화 계층이 추가됨.
- private 속성을 정의하기 복잡할 수 있음 (ES2022 이후 `#`을 사용하여 private 필드를 쉽게 정의 가능).

---

### 비교 요약

| 특성              | 팩토리 함수                          | 생성자 함수                    | 클래스                        |
|-------------------|-----------------------------------|-----------------------------|-----------------------------|
| 사용 방식           | 일반 함수                         | `new` 키워드 사용              | `class` 문법 사용             |
| 메서드 공유         | 불가능 (개별 객체마다 메서드 생성)       | `prototype` 사용             | `prototype` 사용             |
| private 속성 구현    | 클로저 사용 가능                     | 구현 어려움                   | `#` private 필드 지원 (ES2022) |
| 문법 간결성         | 상대적으로 간결함                    | 약간 복잡                     | 가장 직관적, 객체지향 스타일      |
| 상속 구현          | 직접 구현해야 함                     | `prototype`으로 구현         | `extends` 키워드로 간단히 구현 |

---

### 언제 사용해야 할까?

1. **팩토리 함수**:
   - 간단한 객체 생성이 필요할 때.
   - private 속성이나 유연한 객체 반환 로직이 필요할 때.

2. **생성자 함수**:
   - 클래스를 지원하지 않는 오래된 환경에서 객체 생성 및 메서드 공유가 필요할 때.

3. **클래스**:
   - 객체지향 프로그래밍 패턴을 따르며 상속 및 캡슐화를 쉽게 구현하고 싶을 때.
   - 최신 JavaScript 환경에서 간결하고 읽기 쉬운 문법이 필요할 때.

필요에 맞는 방법을 선택하여 JavaScript 객체를 효율적으로 생성하고 관리해 보세요!
