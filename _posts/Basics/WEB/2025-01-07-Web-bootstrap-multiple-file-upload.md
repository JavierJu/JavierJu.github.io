---
title: "Bootstrap을 활용한 다중 파일 업로드 구현: 기본 및 확장 방법"
excerpt: "Bootstrap을 기반으로 다중 파일 업로드를 구현하고, Dropzone.js 및 FilePond.js와 같은 라이브러리를 활용하여 파일 목록 표시와 삭제 기능을 간단하게 추가하는 방법을 알아봅니다."
categories:
  - Web
tags:
  - [Bootstrap, JavaScript, File Upload, Dropzone.js, FilePond.js]
permalink: /web/bootstrap-multiple-file-upload/
toc: true
toc_sticky: true
date: 2025-01-07
last_modified_at: 2025-01-07
---

다중 파일 업로드를 구현할 때, Bootstrap만으로 간단히 시작하거나, 더 강력한 기능을 제공하는 라이브러리인 Dropzone.js와 FilePond.js를 활용하여 효율적으로 구현할 수 있습니다. 이 글에서는 기본적인 파일 업로드 구현부터 고급 라이브러리 사용법까지 다룹니다.

## 기본 Bootstrap 파일 업로드 구현

Bootstrap의 기본 파일 업로드 필드를 활용하여 다중 파일을 업로드하는 가장 간단한 방법입니다.

```html
<div class="mb-3">
    <label for="image" class="form-label">Image File</label>
    <input class="form-control" type="file" id="image" multiple>
</div>
```

위 코드는 간단한 파일 입력 필드를 제공합니다. 하지만 선택한 파일의 목록 표시와 삭제 기능은 별도로 구현해야 합니다.

---

## JavaScript를 사용한 파일 목록 및 삭제 기능 추가

선택한 파일의 이름과 용량을 표시하고 삭제할 수 있도록 JavaScript를 활용한 구현입니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Multiple File Upload</title>
    <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
    <script defer>
        document.addEventListener("DOMContentLoaded", function () {
            const fileInput = document.getElementById("image");
            const fileListContainer = document.getElementById("file-list");

            fileInput.addEventListener("change", function () {
                fileListContainer.innerHTML = ""; // 초기화
                
                Array.from(fileInput.files).forEach((file, index) => {
                    const listItem = document.createElement("div");
                    listItem.className = "d-flex justify-content-between align-items-center mb-2 border rounded p-2";

                    listItem.innerHTML = `
                        <span>
                            ${file.name} (${(file.size / 1024).toFixed(2)} KB)
                        </span>
                        <button type="button" class="btn btn-sm btn-danger remove-file" data-index="${index}">
                            삭제
                        </button>
                    `;

                    fileListContainer.appendChild(listItem);
                });

                // 개별 파일 삭제 처리
                fileListContainer.querySelectorAll(".remove-file").forEach((button) => {
                    button.addEventListener("click", function () {
                        const index = this.getAttribute("data-index");
                        const dt = new DataTransfer();
                        
                        // 현재 파일 목록에서 선택한 파일을 제외
                        Array.from(fileInput.files)
                            .filter((_, i) => i != index)
                            .forEach(file => dt.items.add(file));
                        
                        fileInput.files = dt.files; // 업데이트
                        this.parentElement.remove(); // UI에서 삭제
                    });
                });
            });
        });
    </script>
</head>
<body>
    <div class="container mt-5">
        <form action="/campgrounds" method="post" novalidate class="form-validation" enctype="multipart/form-data">             
            <div class="mb-3">
                <label for="image" class="form-label">Image File: </label>
                <input type="file" name="image" id="image" class="form-control" required multiple>
                <div id="file-list" class="mt-3"></div>
            </div>                
            <div class="mb-3">
                <button type="submit" class="btn btn-success">Submit</button>
            </div>
        </form>
    </div>
</body>
</html>
```

---

## Dropzone.js를 활용한 간단한 파일 업로드

[Dropzone.js](https://www.dropzone.dev/)는 드래그 앤 드롭 업로드와 파일 목록 표시, 삭제 기능을 간단히 구현할 수 있는 라이브러리입니다.

### 사용 방법

1. **CDN 링크 추가**:

```html
<link rel="stylesheet" href="https://unpkg.com/dropzone@5/dist/min/dropzone.min.css" />
<script src="https://unpkg.com/dropzone@5/dist/min/dropzone.min.js"></script>
```

2. **HTML 코드 작성**:

```html
<div class="container mt-5">
    <form action="/campgrounds" class="dropzone" id="my-dropzone">
        <div class="dz-message">Drag and drop files here or click to upload</div>
    </form>
</div>
```

3. **간단한 설정 추가**:

```html
<script>
    Dropzone.options.myDropzone = {
        maxFilesize: 5, // 최대 파일 크기 (MB)
        addRemoveLinks: true, // 삭제 버튼 추가
        dictRemoveFile: "Remove File", // 삭제 버튼 텍스트
        acceptedFiles: "image/*", // 허용 파일 타입
    };
</script>
```

### 결과
- 파일 목록과 삭제 버튼이 자동 생성됩니다.
- 드래그 앤 드롭 기능을 지원합니다.

---

## FilePond.js를 활용한 세련된 UI

[FilePond.js](https://pqina.nl/filepond/)는 직관적인 UI와 강력한 기능을 제공하는 파일 업로드 라이브러리입니다.

### 사용 방법

1. **CDN 링크 추가**:

```html
<link href="https://unpkg.com/filepond/dist/filepond.css" rel="stylesheet">
<script src="https://unpkg.com/filepond/dist/filepond.min.js"></script>
```

2. **HTML 코드 작성**:

```html
<div class="container mt-5">
    <input type="file" class="filepond" name="filepond" multiple />
</div>
```

3. **JavaScript로 활성화**:

```html
<script>
    // FilePond 활성화
    FilePond.create(document.querySelector('.filepond'), {
        allowMultiple: true,
        maxFileSize: '5MB',
        labelIdle: 'Drag & Drop your files or <span class="filepond--label-action">Browse</span>',
    });
</script>
```

### 결과
- 세련된 UI로 다중 파일 업로드를 구현할 수 있습니다.
- 파일 목록 및 삭제 버튼이 자동으로 추가됩니다.

---

## 결론

- **간단한 구현**: Bootstrap과 JavaScript만으로도 파일 목록과 삭제 기능을 추가할 수 있습니다.
- **더 강력한 기능과 쉬운 설정**: Dropzone.js를 사용하면 간단히 드래그 앤 드롭 기능을 추가할 수 있습니다.
- **세련된 UI와 확장 가능성**: FilePond.js를 사용하면 직관적이고 아름다운 파일 업로드 인터페이스를 구현할 수 있습니다.

