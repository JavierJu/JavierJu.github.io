---
title: "[Smart Farm] 외부 IP에서 아두이노의 릴레이를 제어하는 웹 애플리케이션 구축"
excerpt: "외부 IP에서 아두이노의 릴레이를 제어하는 웹 애플리케이션 구축 경험을 공유합니다."

categories:
  - Smart Farm
tags:
  - [Smart Farm, Arduino, Web Development, IoT]

permalink: /Smart-Farm/web-app-arduino/

toc: true
toc_sticky: true

date: 2024-11-10
last_modified_at: 2024-11-10
---

## 프로젝트 개요
현재 외부 IP에서 Wi-Fi에 연결된 아두이노의 릴레이를 원격으로 제어하여 연결된 수중 펌프를 구동하는 시스템을 구축 중입니다. 외부에서 아두이노에 직접 접속할 수 있도록 포트포워딩을 시도했지만, 자택 인터넷이 이중 라우터로 구성되어 있어 고정 IP 설정이 어려운 상황입니다. 이에 대한 대안으로 웹 애플리케이션을 통해 웹 서버에 명령을 기록하고, 아두이노가 주기적으로 웹 서버에서 명령을 읽어와서 동작하는 방식을 채택했습니다.

현재 아두이노는 10초 간격으로 웹 서버에서 명령을 읽어오도록 설정되어 있으며, 테스트 과정에서 주기를 5초 또는 3초로 변경하여 AWS 웹 서버의 부하를 확인하고 있습니다.

## 기능 요구사항
- ON 버튼을 눌렀을 때 타이머를 화면에 표시하고, OFF 버튼을 누르면 타이머가 종료되는 기능을 추가합니다.
- 릴레이 상태를 시각적으로 업데이트하여 현재 동작 상태를 사용자에게 보여줍니다.

## 코드 구현

### `index.html`
아래는 타이머 표시를 위해 수정된 HTML 코드입니다.

```html
<body>
    <header>
        <h1>Water Pump Control Panel</h1>
    </header>
    <main>
        <section>
            <h2 id="relayStatus">Current Relay Status: Loading...</h2>
            <h3 id="timerDisplay">Timer: 0s</h3> <!-- 타이머 표시 영역 -->
        </section>
        <article>
            <div class="button-container">
                <button onclick="controlRelay('ON')" class="RelayTurnOnOff">Turn ON Relay</button>
                <button onclick="controlRelay('OFF')" class="RelayTurnOnOff">Turn OFF Relay</button>
            </div>
        </article>
    </main>
    <script src="script.js"></script>
</body>
```

### `script.js`
릴레이 제어와 타이머 기능을 추가한 JavaScript 코드입니다.

```javascript
let timer; // 타이머 ID
let secondsElapsed = 0; // 경과 시간

function controlRelay(command) {
    const url = `http://IP주소/Controlpump/pump_control.php?command=${command}`;
    fetch(url)
        .then(response => response.text())
        .then(data => {
            alert(`Relay command ${command} executed. Server response: ${data}`);
            updateRelayStatus(command);

            if (command === 'ON') {
                startTimer();
            } else {
                stopTimer();
            }
        })
        .catch(error => {
            alert("Error executing command: " + error);
        });
}

function updateRelayStatus(status) {
    document.getElementById("relayStatus").innerText = `Current Relay Status: ${status}`;
}

function fetchInitialRelayStatus() {
    fetch("http://IP주소/Controlpump/get_latest_command.php")
        .then(response => response.text())
        .then(status => {
            updateRelayStatus(status.trim());
        })
        .catch(error => {
            console.error("Error fetching initial relay status:", error);
        });
}

function startTimer() {
    secondsElapsed = 0;
    document.getElementById("timerDisplay").innerText = `Timer: ${secondsElapsed}s`;
    timer = setInterval(() => {
        secondsElapsed++;
        document.getElementById("timerDisplay").innerText = `Timer: ${secondsElapsed}s`;
    }, 1000);
}

function stopTimer() {
    clearInterval(timer);
    document.getElementById("timerDisplay").innerText = "Timer: 0s";
}
```