---
title: "[JavaScript] fetch API를 사용한 비동기 요청 방식"
excerpt: "fetch API를 사용한 비동기 요청 방식의 구조와 그 역할에 대해 설명"

categories:
  - WEB
tags:
  - [JavaScript, fetch API]

permalink: /WEB/fetch API/

toc: true
toc_sticky: true

date: 2024-10-26
last_modified_at: 2024-10-26
---
 아두이노에 연결된 릴레이를 동작시키기 위해 웹어플리케이션에서 ON/OFF 명령어를 DB 기록하고 현재 상태를 표시하기 위한 로직 생성 중 Fetch API 관련된 내용에 대해 ChatGPT와의 문답을 정리하였다.
 
 예시 코드는 아두이노의 릴레이 작동을 위해 실제 테스트 중인 코드이며 코드 내용 중 등장하는 `pump_control.php`와 `get_latest_command.php`코드는 글 제일 마지막에 참고용으로 추가하였다.

# Fetch API를 사용한 비동기 요청 방식

Fetch API는 JavaScript에서 서버로 데이터를 요청하고 응답을 받을 때 사용하는 최신 비동기 방식입니다. 이전에는 `XMLHttpRequest` 객체를 사용했지만, Fetch API는 코드가 더 간결하고 이해하기 쉬운 방식으로 데이터를 처리할 수 있도록 해줍니다.

## 1. Fetch 함수 개요

- **기능**: Fetch는 URL을 인자로 받아 서버로 요청을 보내고, 이 요청에 대한 응답을 기다립니다.
- **비동기 요청**: Fetch는 비동기 방식으로 실행되어, 요청을 보낸 후에도 코드가 멈추지 않고 계속 실행됩니다.
- **Promise 반환**: Fetch는 Promise 객체를 반환합니다. Promise는 비동기 작업의 결과를 처리하기 위한 객체로, `.then()`과 `.catch()` 메서드를 통해 성공 또는 실패 결과를 처리합니다.

## 2. 코드 예시 설명

아래 코드는 `controlRelay`, `updateRelayStatus`, `fetchInitialRelayStatus`라는 세 가지 주요 함수로 구성되어 있으며, 각 함수는 비동기 요청을 사용해 릴레이 상태를 제어하고 페이지에 표시합니다.

### 1. `controlRelay` 함수

```javascript
function controlRelay(command) {
    const url = `http://ip/Controlpump/pump_control.php?command=${command}`;
    fetch(url)
        .then(response => response.text())
        .then(data => {
            alert(`Relay command ${command} executed. Server response: ${data}`);
            updateRelayStatus(command);  // 명령 후 상태 표시 업데이트
        })
        .catch(error => {
            alert("Error executing command: " + error);
        });
}
```
- **요청 URL 생성**: `command` 변수는 "ON" 또는 "OFF" 값을 받아 URL에 추가됩니다.
- **서버 요청**: `fetch(url)`을 사용해 서버에 요청을 보냅니다.
- **응답 처리**:
  - `.then(response => response.text())`: 응답이 성공적으로 오면, 응답을 텍스트 형식으로 변환합니다.
  - `.then(data => {...})`: 변환된 텍스트 데이터를 받아서 처리합니다. 이 부분에서 alert로 서버 응답을 알리고 `updateRelayStatus(command)`를 호출하여 화면에 상태를 업데이트합니다.
  - `.catch(error => {...})`: 요청이 실패하면 에러를 처리합니다.

### 2. `updateRelayStatus` 함수

이 함수는 릴레이 상태를 표시하는 역할을 하며, 상태가 바뀔 때 페이지에 반영합니다.

```javascript
function updateRelayStatus(status) {
    document.getElementById("relayStatus").innerText = `Current Relay Status: ${status}`;
}
```
relayStatus라는 ID를 가진 HTML 요소의 텍스트 내용을 업데이트하여 현재 릴레이 상태를 보여줍니다.

### 3. `fetchInitialRelayStatus` 함수

이 함수는 페이지가 처음 로드될 때 서버에서 최신 릴레이 상태를 가져와 화면에 표시하는 역할을 합니다.

```javascript
function fetchInitialRelayStatus() {
    fetch("http://ip/Controlpump/get_latest_command.php")
        .then(response => response.text())
        .then(status => {
            updateRelayStatus(status.trim()); // 데이터 표시
        })
        .catch(error => {
            console.error("Error fetching initial relay status:", error);
        });
}
```
이 함수는 fetch 요청을 사용해 `get_latest_command.php` 파일로부터 최신 릴레이 상태를 가져옵니다.

- 페이지 로드 시 자동으로 호출됩니다: `window.onload = fetchInitialRelayStatus;`

## 종합 흐름 요약

- **페이지 로드 시 초기 상태 설정**: `fetchInitialRelayStatus`로 최신 상태를 가져와 표시합니다.
- **버튼을 통한 릴레이 제어**: `controlRelay` 함수가 명령을 서버로 보내고 성공 시 상태를 업데이트합니다.
- **에러 처리**: `.catch()`로 비동기 요청 실패 시 에러를 표시합니다.

이와 같이 Fetch API와 Promise를 사용하면 비동기 요청을 쉽게 관리할 수 있습니다.

`pump_control.php`
```javascript
<?php
$servername = "localhost"; // 데이터베이스 서버
$username = "username"; // MySQL 사용자 이름
$password = "password"; // MySQL 비밀번호
$dbname = "smartfarm_sensordata"; // 사용할 데이터베이스 이름

// 데이터베이스 연결
$conn = new mysqli($servername, $username, $password, $dbname);

// 연결 체크
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if (isset($_GET['command'])) {
    $command = $_GET['command'];

    // pump_commands 테이블에 명령 기록
    $stmt = $conn->prepare("INSERT INTO pump_commands (command) VALUES (?)");
    $stmt->bind_param("s", $command);
    $stmt->execute();

    echo "Relay status updated to: " . $command;
} else {
    // 가장 최근 명령어 읽기
    $result = $conn->query("SELECT command FROM pump_commands ORDER BY created_at DESC LIMIT 1");
    if ($result->num_rows > 0) {
        $row = $result->fetch_assoc();
        echo $row['command'];
    } else {
        echo "No order";
    }
}

// 데이터베이스 연결 종료
$conn->close();
```

`get_latest_command.php`
```javascript
<?php
// MySQL 데이터베이스에 연결
$servername = "localhost";
$username = "username";  // 사용자의 DB 정보
$password = "password";  // 사용자의 DB 비밀번호
$dbname = "smartfarm_sensordata";  // DB 이름

// 연결 생성
$conn = new mysqli($servername, $username, $password, $dbname);

// 연결 확인
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// 최신 명령 조회
$sql = "SELECT command FROM pump_commands ORDER BY id DESC LIMIT 1";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    // 최신 명령 출력
    $row = $result->fetch_assoc();
    echo $row['command'];  // "ON" 또는 "OFF" 반환
} else {
    echo "No order";
}

$conn->close();
```