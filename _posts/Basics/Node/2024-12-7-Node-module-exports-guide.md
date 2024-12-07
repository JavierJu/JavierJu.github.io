---
title: "Node.js의 module.exports: 모듈화와 파일 간 코드 공유"
excerpt: "Node.js의 module.exports에 대해 깊이 알아보고, 이를 활용한 모듈화 및 파일 간 코드 공유 방법을 예제와 함께 설명합니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Backend, 모듈 시스템]
permalink: /Node/module-exports-guide/
toc: true
toc_sticky: true
date: 2024-12-7
last_modified_at: 2024-12-7
---

Node.js에서 `module.exports`는 모듈을 정의하고 내보내는 데 사용되는 핵심 객체입니다. 이를 통해 각 파일을 독립적인 모듈로 다루며, `require`를 통해 다른 파일에서 모듈의 기능을 가져올 수 있습니다. 이 문서에서는 `module.exports`의 원리와 사용 방법을 예제와 함께 살펴봅니다.

## **`module.exports`란?**
Node.js는 CommonJS 모듈 시스템을 사용하여 파일 간 코드 공유를 가능하게 합니다. 각 `.js` 파일은 독립적인 모듈로 간주되며, `module.exports`를 통해 내보낸 객체, 함수, 클래스 등을 다른 파일에서 가져올 수 있습니다.

### **동작 원리**
1. **모듈 정의**: 각 파일은 기본적으로 자체적인 범위를 가지며, 외부에서 접근하려면 `module.exports`로 내보내야 합니다.
2. **모듈 내보내기**: 내보낼 코드를 `module.exports`에 할당합니다.
3. **모듈 가져오기**: 다른 파일에서 `require()`를 사용하여 가져옵니다.

---

## **예제 코드**

### **1. 기본 사용법**
#### `math.js`:
```js
// math.js
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

module.exports = {
    add,
    subtract
};
```

#### `main.js`:
```js
// main.js
const math = require('./math');

console.log(math.add(5, 3));        // 8
console.log(math.subtract(10, 4)); // 6
```

---

### **2. 함수 단독 내보내기**
#### `greet.js`:
```js
// greet.js
module.exports = function(name) {
    return `Hello, ${name}!`;
};
```

#### `main.js`:
```js
// main.js
const greet = require('./greet');

console.log(greet('Alice')); // Hello, Alice!
```

---

### **3. 클래스를 내보내기**
#### `Person.js`:
```js
// Person.js
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    introduce() {
        return `Hi, I'm ${this.name} and I'm ${this.age} years old.`;
    }
}

module.exports = Person;
```

#### `main.js`:
```js
// main.js
const Person = require('./Person');

const alice = new Person('Alice', 25);
console.log(alice.introduce()); // Hi, I'm Alice and I'm 25 years old.
```

---

## **`exports`와의 차이점**
`exports`는 `module.exports`의 별칭(alias)입니다. 하지만 `exports`를 새 객체로 재할당하면 더 이상 `module.exports`와 연결되지 않습니다.

### 올바른 사용:
```js
// math.js
exports.add = function(a, b) {
    return a + b;
};
exports.subtract = function(a, b) {
    return a - b;
};
```

### 잘못된 사용:
```js
// math.js
exports = {
    add: function(a, b) {
        return a + b;
    }
};
// 이 경우, exports는 module.exports와 분리됩니다.
```

---

## **`module.exports`와 NPM의 관계**
- `module.exports`는 NPM(Node Package Manager)과 직접적인 연결은 없지만, NPM 패키지들이 모듈화를 위해 내부적으로 활용합니다.
- 예를 들어, `lodash` 패키지를 사용하면 `module.exports`를 통해 제공된 함수를 사용할 수 있습니다:
```js
const _ = require('lodash');

console.log(_.capitalize('hello world')); // Hello world
```

---

## **결론**
`module.exports`는 Node.js에서 모듈화와 코드 재사용의 핵심 역할을 합니다. 이를 활용하면 파일 간 코드 공유가 간단해지고, 프로젝트의 유지보수성과 가독성이 향상됩니다. Node.js 프로젝트에서 `module.exports`를 올바르게 사용하여 효율적인 코드를 작성해보세요!
