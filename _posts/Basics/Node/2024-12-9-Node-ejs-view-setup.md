---
title: "Express에서 EJS 템플릿 엔진 초기 세팅: views 경로 설정의 중요성"
excerpt: "Express와 EJS를 사용할 때 views 폴더의 경로를 명확히 설정해야 하는 이유와 그 설정 방법을 설명합니다. 실행 위치에 따른 문제를 방지하기 위해 __dirname 을 활용하는 방식도 다룹니다."
categories:
  - Node
tags:
  - [JavaScript, Node.js, Express, EJS, Backend]
permalink: /Node/ejs-view-setup/
toc: true
toc_sticky: true
date: 2024-12-9
last_modified_at: 2024-12-9
---

Express에서 템플릿 엔진으로 EJS를 사용할 때, 다음과 같은 설정이 필요합니다:  

```js
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, '/views'));
```  

위 코드 중 `app.set('views', path.join(__dirname, '/views'));`는 프로젝트를 안정적으로 동작시키기 위해 필수적입니다. 왜 이 설정이 필요한지, 어떤 상황에서 유용한지에 대해 자세히 알아보겠습니다.  

---

### 기본 설정: `views` 디렉토리  
Express는 기본적으로 템플릿 파일이 위치하는 디렉토리를 `views`로 간주합니다.  
이 기본 경로는 현재 작업 디렉토리(`process.cwd()`)를 기준으로 설정됩니다.  

```bash
프로젝트 구조 예시:
/my-project
/views
index.ejs
index.js
```

만약 `/my-project` 디렉토리에서 `node index.js` 명령을 실행하면 `process.cwd()`는 `/my-project`를 반환하므로 Express는 `/my-project/views`에서 템플릿을 찾습니다.  

그러나 다른 디렉토리에서 실행한다면 문제가 발생할 수 있습니다.  
예를 들어 `/outside-folder`에서 아래 명령어를 실행한다고 가정해봅시다:  

```bash
node /my-project/index.js
```  

이 경우, `process.cwd()`는 `/outside-folder`가 됩니다. 따라서 Express는 `/outside-folder/views`를 템플릿 디렉토리로 간주하게 되어 템플릿 파일을 찾을 수 없게 됩니다.  

---

### `__dirname`으로 경로 고정하기  
`__dirname`은 현재 실행 중인 파일(`index.js`)이 위치한 디렉토리의 절대 경로를 반환합니다.  
이를 활용하면 실행 위치에 상관없이 항상 `index.js`를 기준으로 템플릿 파일 경로를 설정할 수 있습니다.  

코드는 다음과 같습니다:  

```js
const path = require('path');
app.set('view engine', 'ejs');
app.set('views', path.join(__dirname, '/views'));
```  

이 설정을 추가하면 템플릿 경로가 실행 파일 위치에 고정되므로 실행 위치와 관계없이 Express가 항상 올바른 디렉토리에서 템플릿 파일을 찾습니다.  

---

### 적용되는 상황  
- **다양한 실행 환경을 지원해야 할 때**  
  예를 들어, 프로젝트 폴더 외부에서 파일을 실행하거나, 실행 위치가 불규칙할 경우 유용합니다.  

- **템플릿 파일 경로가 프로젝트 구조에 고정되어야 할 때**  
  특정 프로젝트 구조를 따르는 경우 안정적인 동작을 보장합니다.  

---

### 요약  
- **문제**: 실행 위치에 따라 Express가 `views` 폴더를 찾지 못할 수 있음.  
- **해결**: `__dirname`과 `path.join`을 활용해 실행 파일 위치를 기준으로 경로를 고정.  
- **결과**: 실행 위치에 관계없이 안정적으로 템플릿 파일을 로드 가능.  

이 설정은 Express 프로젝트에서 템플릿 엔진을 설정할 때 좋은 관행으로 간주되며, 실행 환경에 구애받지 않는 서버 구축에 도움을 줍니다.
