---
title: "JavaScript 객체지향프로그래밍(OOP) 완벽 가이드"
excerpt: "JavaScript에서 객체지향프로그래밍(OOP)의 개념과 주요 특징, 구현 방식을 상세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, 객체지향프로그래밍, OOP, 클래스, 프로토타입]
permalink: /JavaScript/object-oriented-programming-guide/
toc: true
toc_sticky: true
date: 2024-12-6
last_modified_at: 2024-12-6
---

# JavaScript 객체지향프로그래밍(OOP) 완벽 가이드

객체지향 프로그래밍(Object-Oriented Programming, OOP)은 **객체(Object)**라는 개념을 중심으로 설계되고 구현되는 프로그래밍 패러다임입니다. 이 글에서는 JavaScript에서 객체지향프로그래밍의 핵심 개념과 이를 이해하기 위한 내용을 상세히 알아보겠습니다.

---

## 객체(Object)란 무엇인가?
객체는 속성(프로퍼티)과 동작(메서드)을 가진 독립적인 코드 구조입니다.  
JavaScript에서 객체는 `key: value` 형태로 데이터를 구조화합니다.

```js
const car = {
  brand: "Tesla",
  model: "Model 3",
  start() {
    console.log("Car is starting...");
  }
};

console.log(car.brand); // "Tesla"
car.start(); // "Car is starting..."
```

---

## 객체지향 프로그래밍의 주요 특징

### 1. 캡슐화 (Encapsulation)  
객체는 데이터를 안전하게 보호하고, 외부와 상호작용을 위한 인터페이스를 제공합니다.  
속성과 메서드를 객체 내부에 캡슐화하여 **직접 접근을 제한**하고, **메서드를 통해서만 접근**하도록 만듭니다.

```js
class BankAccount {
  #balance = 0; // 비공개 속성

  deposit(amount) {
    if (amount > 0) this.#balance += amount;
  }

  getBalance() {
    return this.#balance; // 외부에서 접근할 수 있는 인터페이스
  }
}

const account = new BankAccount();
account.deposit(100);
console.log(account.getBalance()); // 100
```

### 2. 상속 (Inheritance)  
기존 클래스(또는 객체)의 속성과 동작을 새로운 클래스가 **상속받아 재사용**하거나 확장합니다.

```js
class Animal {
  speak() {
    console.log("Animal makes a sound.");
  }
}

class Dog extends Animal {
  speak() {
    console.log("Dog barks.");
  }
}

const myDog = new Dog();
myDog.speak(); // "Dog barks."
```

### 3. 다형성 (Polymorphism)  
다형성은 같은 인터페이스로 여러 객체의 동작을 호출할 수 있게 합니다.

```js
class Bird {
  move() {
    console.log("Bird flies.");
  }
}

class Fish {
  move() {
    console.log("Fish swims.");
  }
}

function describeMovement(animal) {
  animal.move(); // move 메서드를 호출
}

const bird = new Bird();
const fish = new Fish();

describeMovement(bird); // "Bird flies."
describeMovement(fish); // "Fish swims."
```

### 4. 추상화 (Abstraction)  
복잡한 세부 구현을 숨기고, 중요한 속성이나 동작만 노출합니다.

```js
class Shape {
  area() {
    throw new Error("Method 'area()' must be implemented.");
  }
}

class Circle extends Shape {
  constructor(radius) {
    super();
    this.radius = radius;
  }
  area() {
    return Math.PI * this.radius ** 2; // 세부 구현
  }
}

const circle = new Circle(5);
console.log(circle.area()); // 78.53981633974483
```

---

## JavaScript의 객체지향 프로그래밍 지원 방식

### 1. ES6 클래스 (Class)
JavaScript의 `class` 문법은 OOP를 더욱 직관적으로 작성할 수 있게 합니다.

```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  introduce() {
    console.log(`Hi, my name is ${this.name} and I am ${this.age} years old.`);
  }
}

const john = new Person("John", 30);
john.introduce(); // "Hi, my name is John and I am 30 years old."
```

### 2. 프로토타입 기반 상속
JavaScript는 클래스 대신 프로토타입(prototype)을 기반으로 객체를 상속합니다.

```js
function Person(name, age) {
  this.name = name;
  this.age = age;
}

Person.prototype.introduce = function() {
  console.log(`Hi, my name is ${this.name} and I am ${this.age} years old.`);
};

const jane = new Person("Jane", 25);
jane.introduce(); // "Hi, my name is Jane and I am 25 years old."
```

---

## OOP의 장점
1. **코드 재사용성**: 상속과 모듈화를 통해 중복을 줄이고, 코드를 재사용할 수 있습니다.
2. **유지보수 용이**: 캡슐화 덕분에 내부 구현 변경이 외부에 영향을 미치지 않습니다.
3. **확장성**: 다형성과 상속을 통해 새로운 기능을 추가하거나 기존 기능을 확장하기 쉽습니다.
4. **가독성 향상**: 객체 단위로 코드를 조직화하여 프로그램 구조를 명확히 할 수 있습니다.

---

JavaScript를 이용한 OOP는 유연성과 간결성을 제공합니다. 필요에 따라 적절한 OOP 원칙을 적용해 효율적인 코드를 작성해 보세요.
