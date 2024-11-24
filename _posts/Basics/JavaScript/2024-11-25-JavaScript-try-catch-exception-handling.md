---
title: "JavaScript에서 try/catch 문을 사용하는 이유와 사용법"
excerpt: "JavaScript에서 try/catch 문을 활용하여 오류를 처리하고 예외 상황을 안전하게 관리하는 방법에 대해 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Error Handling, try/catch, Debugging]
permalink: /JavaScript/try-catch-exception-handling/
toc: true
toc_sticky: true
date: 2024-11-25
last_modified_at: 2024-11-25
---

# JavaScript에서 try/catch 문을 사용하는 이유와 사용법

JavaScript에서 `try/catch` 문은 코드 실행 중 발생할 수 있는 오류를 처리하는 데 사용됩니다. 이 구문은 **예외(Exception)**를 처리하는 방식으로 프로그램이 중단되지 않도록 하며, 오류 발생 시 적절한 대응을 할 수 있게 해줍니다.

## `try/catch` 문법

```js
try {
  // 실행할 코드 블록
} catch (error) {
  // 오류가 발생했을 때 실행되는 코드 블록
} finally {
  // (선택 사항) 항상 실행되는 코드 블록
}
```

- **`try` 블록**: 오류가 발생할 가능성이 있는 코드를 작성합니다.
- **`catch` 블록**: `try` 블록에서 오류가 발생하면, 오류를 처리하는 코드가 실행됩니다.
- **`finally` 블록**: 오류가 발생했든 아니든 항상 실행되는 코드입니다.

## 왜 `try/catch`가 필요한가요?

1. **예외 처리**: 오류가 발생해도 애플리케이션이 중단되지 않고 대체 동작을 수행할 수 있습니다.
2. **예측 불가능한 상황 관리**: 외부 API 호출, 사용자 입력 처리, 파일 I/O 등에서 발생할 수 있는 오류를 안전하게 처리할 수 있습니다.
3. **디버깅과 유지보수**: 오류 메시지를 로깅하거나 사용자에게 적절한 피드백을 제공하여 문제를 파악하기 쉽게 만듭니다.

## 사용 사례

### 1. JSON 파싱
사용자가 입력한 JSON 데이터가 올바르지 않거나 오류가 발생한 경우를 처리하는 예제입니다.

```js
const jsonString = '{"name": "John", "age": 30}'; // 올바른 JSON
const invalidJson = '{"name": "John", "age": }';  // 잘못된 JSON

function parseJson(input) {
  try {
    const result = JSON.parse(input);
    console.log("Parsed JSON:", result);
  } catch (error) {
    console.error("JSON parsing error:", error.message);
  } finally {
    console.log("Parsing attempt finished.");
  }
}

parseJson(jsonString); // 정상적으로 파싱
parseJson(invalidJson); // 오류 처리
```

### 2. 외부 API 호출
네트워크 요청 중 오류가 발생했을 때 이를 처리하는 예제입니다.

```js
async function fetchData(url) {
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP Error: ${response.status}`);
    }
    const data = await response.json();
    console.log("Data received:", data);
  } catch (error) {
    console.error("Failed to fetch data:", error.message);
  } finally {
    console.log("Fetch attempt completed.");
  }
}

fetchData("https://api.example.com/data");
```

### 3. 사용자 입력 처리
숫자 계산기에서 잘못된 입력을 처리하는 예제입니다.

```js
function calculateSquareRoot(input) {
  try {
    const number = parseFloat(input);
    if (isNaN(number)) {
      throw new Error("Input is not a valid number.");
    }
    if (number < 0) {
      throw new Error("Cannot calculate square root of a negative number.");
    }
    console.log(`Square root of ${number} is ${Math.sqrt(number)}`);
  } catch (error) {
    console.error("Error:", error.message);
  }
}

calculateSquareRoot("16"); // 정상
calculateSquareRoot("abc"); // 오류: 유효하지 않은 숫자
calculateSquareRoot("-4"); // 오류: 음수
```

## `finally` 블록

`finally`는 오류 발생 여부와 관계없이 항상 실행됩니다. 이 블록은 리소스를 정리하거나 상태를 초기화하는 데 유용합니다.

```js
function processFile(file) {
  let fileHandle;
  try {
    fileHandle = openFile(file); // 파일 열기
    console.log("File processing started.");
    // 파일 처리 로직
  } catch (error) {
    console.error("File processing error:", error.message);
  } finally {
    if (fileHandle) {
      closeFile(fileHandle); // 항상 파일 닫기
    }
    console.log("File processing attempt completed.");
  }
}
```

## `finally` 없이 사용하기

ES10부터는 `finally` 없이 `try` 블록만 사용할 수도 있습니다. 예를 들어, 코드 실행 후 항상 어떤 처리가 필요하다면 `finally`를 생략하고 후속 처리 코드를 직접 작성할 수 있습니다.

```js
try {
  console.log("Try block executed.");
} finally {
  console.log("Finally block executed.");
}
```

## 결론

`try/catch`는 JavaScript에서 예외 처리와 오류 관리를 위한 매우 중요한 도구입니다. 이를 통해 프로그램의 안정성을 높이고 예기치 못한 오류로 인한 중단을 방지할 수 있습니다. 적절한 예외 처리는 코드의 유지보수성을 높이고 디버깅을 쉽게 만들어 주므로, `try/catch`를 활용하는 습관을 가지는 것이 좋습니다.
