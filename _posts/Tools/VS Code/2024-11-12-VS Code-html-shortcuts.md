---
title: "[VS Code] VS Code에서 유용한 HTML 코드 단축키 정리"
excerpt: "VS Code에서 HTML 코드 작성 시 효율성을 높여주는 유용한 단축키들을 정리했습니다. 빠른 코딩을 위한 다양한 단축키를 확인해 보세요."

categories:
  - VS Code
tags:
  - ["VS Code", "단축키", "HTML", "코딩 생산성"]

permalink: /vs-code/html-shortcuts/

toc: true
toc_sticky: true

date: 2024-11-12
last_modified_at: 2024-11-12
---

VS Code는 개발자들이 빠르고 효율적으로 코드를 작성할 수 있도록 다양한 단축키와 기능을 제공합니다. 특히 HTML 코드 작성 시 활용할 수 있는 편리한 단축키들이 있어 생산성을 크게 향상시킬 수 있습니다. 이번 포스팅에서는 자주 사용하는 HTML 단축키들을 정리해 보았습니다.

## 1. 기본 HTML 구조 자동 완성
- **`!` + Tab**: 기본 HTML 문서 구조를 자동으로 생성합니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
    
</body>
</html>
```

## 2. 이미지 태그 자동 완성
- **`img` + Tab**: `<img src="" alt="">` 태그가 자동으로 완성됩니다.

## 3. 클래스명과 ID 자동 완성
- **`.classname` + Tab**: `<div class="classname"></div>` 생성.
- **`#idname` + Tab**: `<div id="idname"></div>` 생성.

## 4. 목록 태그 자동 완성
- **`ul>li*3` + Tab**: 3개의 리스트 아이템이 포함된 `<ul>` 태그를 생성.

```html
<ul>
    <li></li>
    <li></li>
    <li></li>
</ul>
```

- **`ol>li*5` + Tab**: 5개의 리스트 아이템이 포함된 `<ol>` 태그 생성.

## 5. 네스티드 태그 자동 완성
- **`div>p>a` + Tab**: `<div>` 안에 `<p>`가 있고 그 안에 `<a>` 태그가 중첩된 구조 생성.

```html
<div>
    <p><a href=""></a></p>
</div>
```

## 6. 테이블 구조 자동 완성
- **`table>tr*2>td*3` + Tab**: 2개의 행과 각 행에 3개의 셀이 있는 테이블 생성.

```html
<table>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
```

## 7. 주석 자동 완성
- **`Ctrl` + `/`**: 선택한 코드나 커서 위치에 `<!-- -->` 형식으로 HTML 주석을 자동으로 생성.

## 결론
이와 같은 단축키들은 HTML 문서를 빠르게 작성하는 데 큰 도움이 됩니다. VS Code의 Emmet 기능을 통해 가능한 단축키를 잘 활용하면 반복 작업을 줄이고 생산성을 높일 수 있습니다. 꼭 익혀 두어 효율적인 코딩을 해보세요!