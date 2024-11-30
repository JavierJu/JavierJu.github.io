---
title: "새로운 랩탑으로 VS Code 및 GitHub 환경 이전하기"
excerpt: "새로운 랩탑으로 VS Code와 GitHub 환경을 이전하는 방법과 필요한 설정을 안내합니다."
categories:
  - TBD
tags:
  - [VS Code, GitHub, 설정, SSH, 개발 환경]
permalink: /TBD/vscode-github-setup-transfer/
toc: true
toc_sticky: true
date: 2024-11-30
last_modified_at: 2024-11-30
---
  
새로운 랩탑으로 개발 환경을 이전하는 것은 약간의 설정 작업이 필요하지만, 대부분의 작업은 비교적 간단히 이전할 수 있습니다. 이 포스트에서는 VS Code와 GitHub 설정, 원격 SSH 접속 설정을 새로운 컴퓨터로 어떻게 이전할 수 있는지에 대해 정리했습니다.

### 1. VS Code 설정 및 확장 이전하기

VS Code는 사용자의 설정과 확장 프로그램을 로컬 파일로 저장합니다. 이를 이용해 새로운 랩탑으로 환경을 이전할 수 있습니다. 다음은 이전 방법입니다:

#### 설정 파일 이전

VS Code의 설정 파일은 일반적으로 다음 경로에 저장됩니다:

- **Linux**: `~/.config/Code`
- **Windows**: `C:\Users\<사용자>\AppData\Roaming\Code`

이 디렉터리에서 설정 파일(`settings.json`, `keybindings.json` 등)을 찾아서 새로운 랩탑에 복사해 넣으면 기존의 설정을 그대로 사용할 수 있습니다.

#### 확장 프로그램 이전

확장 프로그램을 복사하려면, 아래 명령어로 설치된 확장 목록을 내보낼 수 있습니다:

```bash
code --list-extensions > extensions.txt
```

이후, 새로운 랩탑에서 해당 목록을 사용하여 확장 프로그램을 설치할 수 있습니다:

```bash
cat extensions.txt | xargs -n 1 code --install-extension
```

또는, **VS Code Sync** 기능을 활성화하여 계정을 통해 설정과 확장을 자동으로 동기화할 수 있습니다.

### 2. GitHub 연동 설정

GitHub 연동은 SSH 키나 HTTPS 인증을 통해 이루어집니다. 새로운 랩탑에서 GitHub과 연동하려면 다음 단계를 따릅니다.

#### SSH 키 설정

기존의 SSH 키를 사용하거나, 새로 생성하여 GitHub에 등록할 수 있습니다. SSH 키를 새 랩탑에 복사하려면, `~/.ssh` 디렉터리에서 기존 키 파일을 복사해 주면 됩니다. 새로 키를 생성하려면 다음 명령어를 사용하세요:

```bash
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

그 후, `~/.ssh/id_rsa.pub` 파일의 내용을 GitHub의 **SSH and GPG keys**에 추가하면 됩니다.

### 3. 원격 SSH 접속 설정

VS Code에서 원격 SSH 접속을 설정한 경우, `~/.ssh/config` 파일이나 SSH 연결 정보를 새 랩탑에 복사할 수 있습니다. 또한, `sftp.json` 파일과 같은 원격 서버 설정 파일도 이전해야 합니다.

#### SSH 접속 예시

```html
Host my-remote-server
  HostName example.com
  User user_name
  IdentityFile ~/.ssh/id_rsa
```

이렇게 설정하면 VS Code에서 원격 서버에 접속할 때 사용했던 설정을 그대로 사용할 수 있습니다.

### 4. 기타 파일 및 프로젝트 이전

GitHub에 저장된 프로젝트 파일은 Git을 통해 쉽게 이전할 수 있습니다. 새로운 랩탑에 Git을 설치한 후, 기존 레포지토리를 클론하여 작업을 이어나갈 수 있습니다:

```bash
git clone https://github.com/yourusername/your-repo.git
```

이와 같은 방법으로 새로운 랩탑으로 개발 환경을 손쉽게 이전할 수 있습니다. 대부분의 설정 파일과 프로젝트는 복사하여 사용할 수 있으므로, 빠르게 작업을 재개할 수 있습니다.
