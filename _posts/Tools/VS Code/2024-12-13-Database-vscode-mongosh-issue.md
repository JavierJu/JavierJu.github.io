---
title: "VS Code에서 Mongosh 실행이 안 될 때 해결 방법"
excerpt: "Windows PowerShell에서는 정상 작동하지만 VS Code 터미널에서 Mongosh가 작동하지 않을 때의 주요 원인과 해결책을 알아봅니다."
categories:
  - VS Code
tags:
  - [MongoDB, VSCode, PowerShell, Git Bash, PATH 설정]
permalink: /vs-code/vscode-mongosh-issue/
toc: true
toc_sticky: true
date: 2024-12-13
last_modified_at: 2024-12-13
---

## 문제 상황
Windows PowerShell에서 `mongosh` 명령을 실행하면 정상적으로 작동하지만, VS Code 터미널(PowerShell 또는 Git Bash)에서는 작동하지 않는 경우가 발생할 수 있습니다. 이런 문제는 환경 설정이나 경로 문제에서 비롯됩니다.

## 원인과 해결 방법

### 1. **PATH 설정 확인**
`mongosh` 명령이 인식되지 않는 가장 흔한 이유는 MongoDB의 `bin` 폴더가 시스템 경로(PATH)에 추가되어 있지 않기 때문입니다.

#### PATH 확인 방법:
- PowerShell:
  ```powershell
  $env:PATH
  ```
- Git Bash:
  ```bash
  echo $PATH
  ```
위 명령어로 MongoDB의 경로(`C:\MongoDB\bin`)가 포함되어 있는지 확인합니다.

#### PATH 설정 방법:
- **PowerShell에서 설정:**
  ```powershell
  $env:PATH += ";C:\MongoDB\bin"
  ```
- **Git Bash에서 설정:**
  ```bash
  export PATH="$PATH:/c/MongoDB/bin"
  ```
  
> 💡 영구적으로 설정하려면 시스템 환경 변수에 `C:\MongoDB\bin` 경로를 추가해야 합니다.

---

### 2. **MongoDB 서비스 확인**
MongoDB 서버가 실행 중이 아니면 VS Code에서 `mongosh`가 작동하지 않을 수 있습니다. MongoDB 서버 상태를 확인하려면 다음 명령을 실행하세요:

- PowerShell:
  ```powershell
  Get-Service mongodb
  ```
  
- Git Bash:
  ```bash
  sudo service mongod status
  ```

서버가 실행 중이 아니라면 시작 명령을 사용하세요:

- PowerShell:
  ```powershell
  Start-Service mongodb
  ```
- Git Bash:
  ```bash
  sudo service mongod start
  ```

---

### 3. **VS Code 터미널의 디렉토리 문제**
VS Code 터미널이 MongoDB 관련 파일이 있는 디렉토리가 아닌 다른 경로를 기준으로 실행될 수 있습니다. MongoDB가 설치된 디렉토리로 이동한 뒤 다시 시도하세요:

```bash
cd /c/MongoDB/bin
mongosh
```

---

### 4. **권한 문제**
VS Code에서 실행 시 권한 문제가 발생할 수 있습니다. 관리자 권한으로 VS Code를 실행한 후 다시 시도하세요. 

#### 관리자 권한으로 실행 방법:
- Windows: VS Code 아이콘을 우클릭 → **관리자 권한으로 실행** 선택.

---

### 5. **VS Code 환경 설정 확인**
VS Code의 작업 디렉토리 또는 터미널 환경 설정이 MongoDB의 PATH와 일치하지 않을 수 있습니다. 터미널 환경을 다음과 같이 수정하세요:

#### VS Code의 터미널 환경 설정:
1. **설정 파일 열기**:
   `Ctrl + ,`를 눌러 설정 창 열기 → "터미널 > 환경 변수" 검색.
2. **환경 변수 추가**:
   `PATH`에 `C:\MongoDB\bin` 추가.

---

### 결론
위 방법을 하나씩 시도하며 `mongosh`가 VS Code 터미널에서도 작동하도록 설정하세요. 문제가 지속되면 환경 변수나 MongoDB 설치 디렉토리를 다시 확인하거나, VS Code 터미널 대신 외부 PowerShell에서 계속 사용하는 것도 고려할 수 있습니다.
