---
title: "[Smart Farm] Webapplication for Arduino sensor data"
excerpt: "테스트"

categories:
  - Smart Farm
tags:
  - [Smart Farm, test]

permalink: /Smart Farm/posting-test2/

toc: true
toc_sticky: true

date: 2024-10-28
last_modified_at: 2024-10-28
---

외부 IP에서 와이파이에 연결된 아두이노의 릴레이를 원격으로 제어해 연결된 수중펌프를 구동하는 시스템을 구축 중이다. 포트포워딩을 통해 외부에서 직접 아두이노에 연결이 가능하다고 하지만 현재 자택 인터넷이 이중 라우터로 구성되어 있어 고정 IP 설정이 어려운 상황이다. 그래서 대안으로 웹 어플리케이션을 통해 웹서버에 명령을 기록하고 아두이노에서 웹서버에서 명령을 주기적으로 읽어와서 구동하는 방식을 테스트 중이다. 현행 방식으로는 명령 후 아두이노가 웹서버에서 10초 주기로 읽어 오게 셋팅을 하고 있어 -> 5초, 3초 등 테스트할 경우 AWS 웹서버 부하 확인하기



릴레이 제어 코드인데, ON 버튼을 누를 경우 화면 밑에 타이머도 추가하고 싶어. ON 버튼을 누르고 몇초간 실행하는걸 시각적으로 보고 싶은데. OFF 를 누르면 타이머는 종료되고.

index.html
<body>
    <header>
        <h1>Water Pump Control Panel</h1>
    </header>
    <main>
        <section>
            <h2 id="relayStatus">Current Relay Status: Loading...</h2>
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

script.js
// 릴레이제어
function controlRelay(command) {
    const url = http://13.208.254.200/Controlpump/pump_control.php?command=${command};
    fetch(url)
        .then(response => response.text())
        .then(data => {
            alert(Relay command ${command} executed. Server response: ${data});
            updateRelayStatus(command);  // 명령 후 상태 표시 업데이트
        })
        .catch(error => {
            alert("Error executing command: " + error);
        });
}

function updateRelayStatus(status) {
    document.getElementById("relayStatus").innerText = Current Relay Status: ${status};
}

function fetchInitialRelayStatus() {
    fetch("http://13.208.254.200/Controlpump/get_latest_command.php")
        .then(response => response.text())  // 텍스트로 응답 파싱
        .then(status => {
            updateRelayStatus(status.trim()); // 데이터 표시
        })
        .catch(error => {
            console.error("Error fetching initial relay status:", error);
        });
}

window.onload = fetchInitialRelayStatus;  // 페이지 로드시 초기 상태 불러오기

ChatGPT의 말:
ChatGPT
ON 버튼을 눌렀을 때 타이머를 화면에 표시하고, OFF 버튼을 누르면 타이머를 종료하도록 구현할 수 있습니다. 아래 코드는 타이머를 시작하고 업데이트하는 로직을 추가하여, ON 버튼을 눌렀을 때 몇 초 동안 실행되는지를 시각적으로 표시하도록 구성한 예시입니다.

수정된 코드
index.html - 타이머 표시 영역을 추가합니다.
html
코드 복사
<body>
    <header>
        <h1>Water Pump Control Panel</h1>
    </header>
    <main>
        <section>
            <h2 id="relayStatus">Current Relay Status: Loading...</h2>
        </section>
        <article>
            <div class="button-container">
                <button onclick="controlRelay('ON')" class="RelayTurnOnOff">Turn ON Relay</button>
                <button onclick="controlRelay('OFF')" class="RelayTurnOnOff">Turn OFF Relay</button>
            </div>
        </article>
        <section>
            <h3 id="timerDisplay">Timer: 0 seconds</h3> <!-- 타이머 표시 -->
        </section>
    </main>
    <script src="script.js"></script>
</body>
script.js - 타이머 로직을 추가합니다.
javascript
코드 복사
let timer; // 타이머 간격 저장
let seconds = 0; // 타이머 카운트 저장

// 릴레이 제어
function controlRelay(command) {
    const url = `http://13.208.254.200/Controlpump/pump_control.php?command=${command}`;
    fetch(url)
        .then(response => response.text())
        .then(data => {
            alert(`Relay command ${command} executed. Server response: ${data}`);
            updateRelayStatus(command);  // 명령 후 상태 표시 업데이트

            if (command === 'ON') {
                startTimer(); // ON 버튼을 누르면 타이머 시작
            } else {
                stopTimer();  // OFF 버튼을 누르면 타이머 종료
            }
        })
        .catch(error => {
            alert("Error executing command: " + error);
        });
}

// 타이머 시작 함수
function startTimer() {
    seconds = 0; // 초기화
    updateTimerDisplay();
    timer = setInterval(() => {
        seconds++;
        updateTimerDisplay();
    }, 1000); // 1초마다 업데이트
}

// 타이머 종료 함수
function stopTimer() {
    clearInterval(timer); // 타이머 중지
    document.getElementById("timerDisplay").innerText = "Timer: Stopped"; // 타이머 종료 메시지
}

// 타이머 표시 업데이트
function updateTimerDisplay() {
    document.getElementById("timerDisplay").innerText = `Timer: ${seconds} seconds`;
}

// 릴레이 상태 업데이트
function updateRelayStatus(status) {
    document.getElementById("relayStatus").innerText = `Current Relay Status: ${status}`;
}

// 초기 릴레이 상태 가져오기
function fetchInitialRelayStatus() {
    fetch("http://13.208.254.200/Controlpump/get_latest_command.php")
        .then(response => response.text())  // 텍스트로 응답 파싱
        .then(status => {
            updateRelayStatus(status.trim()); // 데이터 표시
        })
        .catch(error => {
            console.error("Error fetching initial relay status:", error);
        });
}

window.onload = fetchInitialRelayStatus;  // 페이지 로드시 초기 상태 불러오기
설명
startTimer 함수: ON 버튼을 누르면 타이머를 1초마다 증가시키며 화면에 업데이트합니다.
stopTimer 함수: OFF 버튼을 누르면 타이머를 중지하고 “Timer: Stopped”로 표시합니다.
updateTimerDisplay 함수: 타이머 값을 갱신해 timerDisplay 요소에 표시합니다.
이제 ON 버튼을 누르면 타이머가 시작되고, OFF 버튼을 누르면 타이머가 중지됩니다.


데스크톱 크롬 웹페이지에서는 타이머가 정상 작동되는데, 스마트폰 크롬페이지에서는 ON/OFF 작동은 문제 없는데 타이머가 0 에서 변화가 없는데

ChatGPT의 말:
ChatGPT
모바일에서 타이머가 0에서 멈추는 현상은 setInterval이 제대로 동작하지 않거나 타이머 업데이트가 스마트폰 환경에서 제한을 받는 경우 발생할 수 있습니다. 모바일 브라우저는 배터리 절약 등을 위해 setInterval의 실행 빈도를 줄이는 경우가 있습니다. 이를 해결하기 위해 타이머를 Date 객체를 사용하여 구현하면 더 정확한 시간 계산이 가능합니다.

수정된 코드
startTimer 함수와 stopTimer 함수를 아래와 같이 변경해 보세요.

javascript
코드 복사
let timer;
let startTime;

// 타이머 시작 함수 (Date 객체로 시간 계산)
function startTimer() {
    startTime = Date.now();  // 시작 시간 저장
    updateTimerDisplay();
    timer = setInterval(() => {
        const currentTime = Date.now();
        const elapsedSeconds = Math.floor((currentTime - startTime) / 1000);
        document.getElementById("timerDisplay").innerText = `Timer: ${elapsedSeconds} seconds`;
    }, 1000);
}

// 타이머 종료 함수
function stopTimer() {
    clearInterval(timer);
    document.getElementById("timerDisplay").innerText = "Timer: Stopped";
}
설명
startTime: Date.now()로 시작 시간을 기록합니다.
setInterval: startTime에서 경과한 시간을 elapsedSeconds로 계산하여, 1초마다 timerDisplay에 업데이트합니다.
이 방식은 모바일에서도 더 정확하게 타이머를 유지할 수 있습니다.
