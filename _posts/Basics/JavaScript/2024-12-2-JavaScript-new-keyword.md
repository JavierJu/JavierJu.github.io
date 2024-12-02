---  
title: "JavaScript의 new 키워드: 객체 생성 원리와 사용 방법"  
excerpt: "new 키워드를 활용하여 객체를 생성하는 원리와 사용 사례를 알아봅니다."  
categories:  
  - JavaScript  
tags:  
  - [JavaScript, 객체지향, 클래스, 프로토타입, 생성자]  
permalink: /JavaScript/new-keyword/  
toc: true  
toc_sticky: true  
date: 2024-12-2  
last_modified_at: 2024-12-2 
---  

## `new` 키워드란?  
JavaScript에서 `new`는 객체를 생성할 때 사용하는 키워드입니다. 주로 클래스 또는 생성자 함수를 사용하여 새로운 객체를 만들 때 활용됩니다.  

### `new` 키워드의 동작 원리  
`new`는 다음과 같은 단계를 거쳐 객체를 생성합니다:  

1. **새로운 빈 객체 생성**  
   `new`를 호출하면 JavaScript 엔진은 새로운 빈 객체를 생성합니다.  

2. **객체의 프로토타입 설정**  
   생성된 객체는 호출된 클래스나 생성자 함수의 `prototype` 속성을 상속받습니다.  

3. **생성자 함수 실행**  
   생성자 함수(또는 클래스의 `constructor` 메서드)가 실행되며, 이 함수 내에서 `this`는 방금 생성된 객체를 가리킵니다.  

4. **객체 반환**  
   생성자 함수가 명시적으로 객체를 반환하지 않으면 새로 생성된 객체가 반환됩니다.  

### 예제: 클래스 사용  
다음은 `new` 키워드를 사용해 클래스로 객체를 생성하는 예제입니다:  

``` js  
class Person {  
  constructor(name, age) {  
    this.name = name;  
    this.age = age;  
  }  

  greet() {  
    console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);  
  }  
}  

const person1 = new Person("Alice", 30);  // Person 객체를 생성  
person1.greet();  // "Hello, my name is Alice and I am 30 years old."  
```  

### `new` 없이 생성자 함수를 호출하면?  
`new` 없이 생성자 함수를 호출하면 예기치 않은 동작이 발생할 수 있습니다.  

예를 들어:  

``` js  
function Person(name, age) {  
  this.name = name;  
  this.age = age;  
}  

const person1 = Person("Alice", 30); // new 없이 호출  
console.log(name);  // "Alice" (전역 변수에 할당됨)  
```  

위의 코드는 `new` 없이 호출되었기 때문에 `this`가 글로벌 객체(`window` 또는 `global`)를 가리키게 됩니다. 결과적으로 `name`이 전역 변수로 설정됩니다.  

### 언제 `new`를 사용해야 하나요?  
- **클래스 인스턴스 생성**: 클래스에서 객체를 생성할 때 사용합니다.  
- **생성자 함수 호출**: 전통적인 방식의 생성자 함수에서도 `new`는 필수입니다.  

### 요약  
`new` 키워드는 객체 지향 프로그래밍에서 새로운 객체를 생성하는 데 필수적으로 사용됩니다. 이를 올바르게 이해하고 사용하면 코드의 가독성과 안정성을 높일 수 있습니다.  
