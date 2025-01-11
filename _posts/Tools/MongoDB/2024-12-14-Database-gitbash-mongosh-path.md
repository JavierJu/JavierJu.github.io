---

title: "Git Bash에서 mongosh PATH 설정 문제 해결하기" 
excerpt: "Git Bash에서 MongoDB와 mongosh를 매번 수동으로 설정해야 하는 문제를 해결하고, PATH를 영구적으로 추가하는 방법을 알아봅니다." 
categories:
- MongoDB
VS Code tags:
- [Git Bash, MongoDB, 환경 변수, VS Code] 
permalink: /mongodb/gitbash-mongosh-path/ 
toc: true 
toc_sticky: true 
date: 2024-12-14 
last_modified_at: 2024-12-14

---

## 문제 상황

Windows PowerShell에서는 환경 변수 PATH에 MongoDB 경로를 추가하면 `mongosh` 명령이 정상적으로 동작합니다. 하지만 Git Bash에서는 매번 아래 명령어를 수동으로 실행해야 `mongosh`를 사용할 수 있습니다:

```bash
$ export PATH="$PATH:/c/MongoDB/bin"
$ export PATH="$PATH:/c/MongoDB/mongosh"
```

왜 이런 문제가 발생하며, 어떻게 해결할 수 있을까요?

---

## 원인 분석

### 1. **Git Bash의 PATH 처리 방식**

Git Bash는 `/etc/profile`, `~/.bash_profile`, `~/.bashrc`와 같은 초기화 스크립트를 사용하여 환경 변수를 설정합니다. Windows 환경 변수 PATH에 추가된 값은 Git Bash가 완벽히 가져오지 않을 수 있습니다.

### 2. **세션 종료 시 PATH 초기화**

`export PATH` 명령은 **현재 터미널 세션에서만 유효**합니다. 따라서 Git Bash를 새로 열 때마다 설정이 초기화되어 매번 같은 명령을 실행해야 합니다.

### 3. **VS Code 터미널에서의 추가 문제**

VS Code 터미널도 Git Bash의 환경 설정을 따르기 때문에, PATH 설정이 지속되지 않을 경우 동일한 문제가 발생합니다.

---

## 해결 방법

Git Bash에서 매번 `export PATH`를 실행하지 않도록 환경 변수를 영구적으로 설정하는 방법은 다음과 같습니다.

### 방법 1: `~/.bashrc` 또는 `~/.bash_profile`에 경로 추가

1. Git Bash를 실행합니다.

2. 홈 디렉터리에 위치한 설정 파일을 엽니다:

   ```bash
   nano ~/.bashrc
   ```

   또는

   ```bash
   nano ~/.bash_profile
   ```

3. 파일 끝에 아래 내용을 추가합니다:

   ```bash
   export PATH="$PATH:/c/MongoDB/bin"
   export PATH="$PATH:/c/MongoDB/mongosh"
   ```

4. 파일을 저장하고 닫습니다.

5. 변경 사항을 현재 세션에 적용합니다:

   ```bash
   source ~/.bashrc
   ```

   또는

   ```bash
   source ~/.bash_profile
   ```

6. Git Bash를 재시작한 뒤, `mongosh`가 정상적으로 실행되는지 확인합니다.

---

### 방법 2: Git Bash 공용 설정 파일 수정

Git Bash 전체에 적용하려면 `/etc/profile` 파일을 수정합니다:

1. 공용 설정 파일을 엽니다:

   ```bash
   nano /etc/profile
   ```

2. 아래 내용을 추가합니다:

   ```bash
   export PATH="$PATH:/c/MongoDB/bin"
   export PATH="$PATH:/c/MongoDB/mongosh"
   ```

3. 파일을 저장한 뒤, 변경 사항을 적용합니다:

   ```bash
   source /etc/profile
   ```

4. Git Bash를 재시작하여 `mongosh`가 정상적으로 동작하는지 확인합니다.

---

### 방법 3: Windows 환경 변수 동기화 확인

1. Windows 환경 변수 설정에서 MongoDB 경로가 추가되었는지 확인합니다:

   - **경로**: `C:\MongoDB\bin` 및 `C:\MongoDB\mongosh`

2. Git Bash가 이를 반영하지 않는다면, 위의 `.bashrc` 또는 `/etc/profile` 설정 방법을 사용해야 합니다.

---

## 참고 사항

- 이 설정은 Git Bash뿐만 아니라 VS Code에서 Git Bash 터미널을 사용할 때도 적용됩니다.
- 경로를 추가한 뒤, 터미널을 새로 열 때마다 자동으로 적용되므로 더 이상 수동으로 `export PATH`를 실행할 필요가 없습니다.

이제 Git Bash에서도 MongoDB와 `mongosh`를 문제없이 사용할 수 있습니다!

