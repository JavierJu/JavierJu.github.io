---
title: "multipart/form-data: 파일 업로드와 폼 데이터 전송의 이해"
excerpt: "웹 애플리케이션에서 파일 업로드와 텍스트 데이터를 동시에 전송하는 multipart/form-data의 구조와 작동 방식을 알아봅니다."
categories:
  - Web
tags:
  - [HTML, HTTP, 파일 업로드, multipart, 웹 개발]
permalink: /web/multipart-form-data/
toc: true
toc_sticky: true
date: 2025-01-07
last_modified_at: 2025-01-07
---

## multipart/form-data란?

`multipart/form-data`는 웹 애플리케이션에서 **파일 업로드**와 같은 바이너리 데이터와 텍스트 데이터를 함께 전송할 때 사용되는 HTTP 요청의 **인코딩 방식**입니다. 이는 HTML 폼에서 `<form>` 태그의 `enctype` 속성에 설정되며, 파일과 텍스트 데이터를 효율적으로 전송하도록 설계되었습니다.

---

### 주요 특징

1. **다중 파트(Multipart)**:
   - 데이터를 여러 부분(part)으로 나누어 전송하며, 각 파트는 고유한 헤더와 데이터를 포함합니다.
   - 텍스트 데이터와 바이너리 데이터를 함께 포함할 수 있습니다.

2. **바운더리(boundary) 사용**:
   - 각 파트를 구분하기 위해 고유한 문자열(바운더리)이 사용됩니다.
   - 서버는 이 바운더리를 기준으로 데이터를 구분하여 처리합니다.

3. **효율적인 바이너리 전송**:
   - 파일 같은 바이너리 데이터를 Base64로 인코딩하지 않고 원본 그대로 전송하므로 효율적입니다.

4. **HTTP POST 요청에서 주로 사용**:
   - 특히 파일 업로드 기능이 포함된 폼 데이터 전송에 적합합니다.

---

## 사용 예시

### HTML 폼 예시

```html
<form action="/upload" method="POST" enctype="multipart/form-data">
    <input type="text" name="username">
    <input type="file" name="file">
    <button type="submit">Upload</button>
</form>
```

### 전송되는 HTTP 요청 (요약)

```http
POST /upload HTTP/1.1
Content-Type: multipart/form-data; boundary=----WebKitFormBoundaryxYz12345

------WebKitFormBoundaryxYz12345
Content-Disposition: form-data; name="username"

john_doe
------WebKitFormBoundaryxYz12345
Content-Disposition: form-data; name="file"; filename="example.txt"
Content-Type: text/plain

This is the content of the file.
------WebKitFormBoundaryxYz12345--
```

### 요청 설명

1. `Content-Type` 헤더는 `multipart/form-data`와 바운더리를 포함합니다.
2. 데이터는 바운더리(`------WebKitFormBoundaryxYz12345`)로 구분됩니다.
3. 각 파트는 `Content-Disposition` 헤더를 포함하며, 데이터의 **필드 이름**과 **파일 이름**(파일의 경우)을 지정합니다.
4. 파일 데이터는 원본 그대로 전송됩니다.

---

## 주요 헤더

1. **Content-Type**:
   - `multipart/form-data; boundary=----boundary_string`
   - 바운더리(`boundary_string`)는 서버가 데이터를 파트별로 구분할 수 있게 합니다.

2. **Content-Disposition**:
   - 각 파트의 데이터를 설명하며, 주로 `form-data`, `name`(필드 이름), `filename`(파일 이름) 속성을 포함합니다.

3. **Content-Type (옵션)**:
   - 파일 데이터의 MIME 타입을 나타냅니다 (예: `image/png`, `text/plain`).

---

## 작동 방식

1. **브라우저**:
   - HTML 폼 데이터를 수집하고 `multipart/form-data` 형식으로 인코딩합니다.
   - 각 입력 필드를 별도의 파트로 나누고, 각 파트를 바운더리로 구분하여 HTTP 요청 본문에 포함합니다.

2. **서버**:
   - 요청의 `Content-Type` 헤더를 확인하고 `multipart/form-data` 형식인지 판별합니다.
   - 바운더리를 기준으로 데이터를 파트별로 구분하고 각 파트의 내용을 처리합니다.

---

## 주의점 및 장점

### 장점
- 텍스트와 바이너리 데이터를 동시에 처리 가능.
- 바이너리 데이터를 원본 그대로 전송하므로 효율적.
- 다양한 데이터 형식을 포함할 수 있어 유연성 제공.

### 주의점
- 요청 크기가 커질 수 있으므로 서버에서 적절히 처리해야 합니다.
- 업로드된 파일은 서버에서 저장 또는 별도 처리 로직이 필요합니다.
- 파일 업로드 시 보안(예: 파일 크기 제한, 파일 형식 검증 등)에 신경 써야 합니다.

---

## 실제 사용

### 서버에서 데이터 처리 (Node.js 예시)

```javascript
const express = require('express');
const multer = require('multer');
const app = express();

const upload = multer({ dest: 'uploads/' });

app.post('/upload', upload.single('file'), (req, res) => {
    console.log(req.body); // 텍스트 데이터
    console.log(req.file); // 업로드된 파일 정보
    res.send('File uploaded successfully');
});

app.listen(3000, () => {
    console.log('Server started on http://localhost:3000');
});
```

---

`multipart/form-data`는 파일 업로드와 같은 데이터 전송을 간단하고 효율적으로 처리할 수 있도록 해줍니다. 😊

