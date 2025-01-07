---
title: "Multer: Node.js에서 파일 업로드를 처리하는 방법"
excerpt: "Multer를 활용한 파일 업로드의 기본 설정부터 고급 사용법까지 살펴봅니다. 파일 저장, 필터링, 크기 제한 등을 포함한 실용적인 예제를 제공합니다."
categories:
  - Node
  - Express
  - Middleware
tags:
  - [Node.js, Express, Multer, 파일 업로드, 백엔드]
permalink: /node/multer-guide/
toc: true
toc_sticky: true
date: 2025-01-07
last_modified_at: 2025-01-07
---

## Multer란?

Multer는 Node.js의 웹 프레임워크인 **Express**에서 **multipart/form-data**를 처리하기 위한 미들웨어입니다. `multipart/form-data`는 파일 업로드와 같은 폼 데이터를 전송할 때 사용하는 인코딩 방식으로, Multer는 이를 쉽게 처리하도록 도와줍니다.

Multer는 **파일 업로드**를 관리하며, 업로드된 파일의 저장 위치, 파일 이름 지정, 필터링 등을 커스터마이징할 수 있는 기능을 제공합니다.

---

## Multer의 주요 특징

1. **파일 처리 전용**:
   - Multer는 `multipart/form-data`에서 파일을 처리합니다.
   - 텍스트 필드 데이터도 함께 처리할 수 있습니다.

2. **스토리지 관리**:
   - Multer는 업로드된 파일을 메모리나 디스크에 저장할 수 있습니다.
   - 기본적으로 디스크 스토리지와 메모리 스토리지를 제공합니다.

3. **사용자 정의 가능**:
   - 파일의 저장 경로, 파일 이름, 업로드 조건 등을 설정할 수 있습니다.

4. **간편한 통합**:
   - Express 애플리케이션에서 쉽게 통합하여 사용할 수 있습니다.

---

## Multer 설치

```bash
npm install multer
```

---

## 기본 사용법

### 1. 단일 파일 업로드

아래는 단일 파일을 업로드하는 예제입니다.

```javascript
const express = require('express');
const multer = require('multer');
const app = express();

// 파일 저장 설정
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    cb(null, 'uploads/'); // 파일이 저장될 디렉토리
  },
  filename: function (req, file, cb) {
    cb(null, Date.now() + '-' + file.originalname); // 고유한 파일 이름 생성
  },
});

// Multer 설정
const upload = multer({ storage: storage });

// 단일 파일 업로드 라우트
app.post('/upload', upload.single('file'), (req, res) => {
  res.send('파일 업로드 완료!');
});

app.listen(3000, () => {
  console.log('서버가 3000번 포트에서 실행 중입니다.');
});
```

### 2. 다중 파일 업로드

```javascript
app.post('/upload-multiple', upload.array('files', 5), (req, res) => {
  // 최대 5개의 파일 업로드
  res.send('여러 파일 업로드 완료!');
});
```

---

## 옵션 및 설정

### 1. **Storage 설정**

Multer는 파일을 저장할 위치와 파일 이름을 설정할 수 있도록 두 가지 스토리지 엔진을 제공합니다.

- **DiskStorage**:
  업로드된 파일을 디스크에 저장합니다.

  ```javascript
  const storage = multer.diskStorage({
    destination: function (req, file, cb) {
      cb(null, 'uploads/');
    },
    filename: function (req, file, cb) {
      cb(null, Date.now() + '-' + file.originalname);
    },
  });
  ```

- **MemoryStorage**:
  업로드된 파일을 메모리에 버퍼로 저장합니다.

  ```javascript
  const storage = multer.memoryStorage();
  ```

### 2. **File 필터링**

업로드 가능한 파일의 종류를 제한할 수 있습니다.

```javascript
const upload = multer({
  storage: storage,
  fileFilter: function (req, file, cb) {
    if (file.mimetype === 'image/png' || file.mimetype === 'image/jpeg') {
      cb(null, true); // 파일 허용
    } else {
      cb(new Error('이미지 파일만 업로드 가능합니다.'), false); // 파일 거부
    }
  },
});
```

### 3. **파일 크기 제한**

업로드 파일 크기를 제한할 수 있습니다.

```javascript
const upload = multer({
  storage: storage,
  limits: { fileSize: 1024 * 1024 * 5 }, // 5MB 제한
});
```

---

## 요청 데이터 처리

- **파일 데이터**:
  업로드된 파일 데이터는 `req.file`(단일 파일) 또는 `req.files`(다중 파일)로 접근할 수 있습니다.

- **폼 필드 데이터**:
  다른 폼 필드 데이터는 `req.body`로 접근할 수 있습니다.

```javascript
app.post('/upload', upload.single('file'), (req, res) => {
  console.log(req.file); // 업로드된 파일 정보
  console.log(req.body); // 폼 데이터
  res.send('업로드 완료');
});
```

---

## 에러 처리

Multer는 자체 에러를 발생시킬 수 있으며, 이를 처리하려면 일반 Express 에러 핸들러를 사용합니다.

```javascript
app.post('/upload', (req, res, next) => {
  upload.single('file')(req, res, (err) => {
    if (err instanceof multer.MulterError) {
      // Multer 관련 에러 처리
      return res.status(400).send(err.message);
    } else if (err) {
      // 기타 에러 처리
      return res.status(500).send(err.message);
    }
    next();
  });
});
```

---

## 응용 예제

### 1. 이미지 업로드 후 클라이언트에 반환

```javascript
app.post('/upload-image', upload.single('image'), (req, res) => {
  const filePath = `/uploads/${req.file.filename}`;
  res.json({ filePath });
});
```

### 2. 특정 사용자별 디렉토리 저장

```javascript
const storage = multer.diskStorage({
  destination: function (req, file, cb) {
    const userId = req.body.userId;
    const dir = `uploads/${userId}`;
    cb(null, dir);
  },
  filename: function (req, file, cb) {
    cb(null, file.originalname);
  },
});
```

---

## 주요 참고 사항

1. Multer는 파일의 유효성 검사를 기본적으로 제공하지 않습니다. 파일 업로드 후 추가 검증을 수행해야 합니다.
2. 메모리 스토리지를 사용할 때는 메모리 사용량을 주의해야 합니다.
3. 보안 문제를 방지하기 위해 업로드된 파일을 처리한 후 반드시 저장 위치를 관리해야 합니다.

---

Multer는 파일 업로드를 간단하고 효과적으로 처리할 수 있도록 설계된 라이브러리로, Express 기반 애플리케이션에서 필수적으로 사용됩니다.

