---
title: "Express.js에서 npm sitemap으로 동적 Sitemap 생성하기"
excerpt: "Express.js에서 sitemap 패키지를 활용하여 동적으로 Sitemap을 생성하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - Express.js
  - Backend
  - SEO
  - Node
tags:
  - [Express.js, JavaScript, Backend, SEO, Sitemap]
permalink: /node/dynamic-sitemap/
toc: true
toc_sticky: true
date: 2025-02-18
last_modified_at: 2025-02-18
---

## Express.js에서 `npm sitemap`을 사용하여 Sitemap 생성하기

Sitemap(사이트맵)은 검색 엔진이 웹사이트를 크롤링하고 색인화하는 데 도움을 주는 XML 파일입니다. Express.js에서 `sitemap` 패키지를 사용하면 동적으로 Sitemap을 생성할 수 있습니다.

---

## 1. `sitemap` 패키지 설치

먼저 프로젝트에서 `sitemap` 패키지를 설치해야 합니다.

```sh
npm install sitemap
```

---

## 2. Express에서 Sitemap 생성

### 📌 예제: 기본적인 Sitemap 생성

다음은 Express.js에서 동적으로 Sitemap을 생성하는 기본적인 코드입니다.

```javascript
const express = require("express");
const { SitemapStream, streamToPromise } = require("sitemap");
const { createGzip } = require("zlib");

const app = express();

app.get("/sitemap.xml", async (req, res) => {
  try {
    res.header("Content-Type", "application/xml");
    res.header("Content-Encoding", "gzip");

    const smStream = new SitemapStream({ hostname: "https://yourwebsite.com" });
    const pipeline = smStream.pipe(createGzip());

    // Sitemap에 추가할 경로들
    smStream.write({ url: "/", changefreq: "daily", priority: 1.0 });
    smStream.write({ url: "/about", changefreq: "weekly", priority: 0.8 });
    smStream.write({ url: "/contact", changefreq: "monthly", priority: 0.5 });

    smStream.end();

    streamToPromise(pipeline).then((sitemap) => res.send(sitemap));
  } catch (error) {
    console.error(error);
    res.status(500).end();
  }
});

const PORT = 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

### 🔍 코드 설명:

1. `SitemapStream`을 생성하여 XML 문서를 스트리밍 방식으로 작성합니다.
2. `gzip` 압축을 사용하여 파일 크기를 줄입니다.
3. `smStream.write()`를 사용하여 사이트의 URL을 추가합니다.
4. `streamToPromise()`로 변환하여 클라이언트에 반환합니다.

---

## 3. 동적 Sitemap 생성 (예: 데이터베이스에서 페이지 URL 가져오기)

만약 블로그나 게시글과 같은 동적 URL을 포함해야 한다면, 데이터베이스에서 가져온 데이터를 활용할 수 있습니다.

```javascript
const express = require("express");
const { SitemapStream, streamToPromise } = require("sitemap");
const { createGzip } = require("zlib");

// 예제 데이터 (실제로는 DB에서 가져와야 함)
const blogPosts = [
  { slug: "post-1", lastmod: "2025-02-01" },
  { slug: "post-2", lastmod: "2025-02-10" },
];

const app = express();

app.get("/sitemap.xml", async (req, res) => {
  try {
    res.header("Content-Type", "application/xml");
    res.header("Content-Encoding", "gzip");

    const smStream = new SitemapStream({ hostname: "https://yourwebsite.com" });
    const pipeline = smStream.pipe(createGzip());

    // 정적 페이지 추가
    smStream.write({ url: "/", changefreq: "daily", priority: 1.0 });
    smStream.write({ url: "/about", changefreq: "weekly", priority: 0.8 });

    // 동적 URL 추가
    blogPosts.forEach((post) => {
      smStream.write({
        url: `/blog/${post.slug}`,
        lastmodISO: post.lastmod,
        changefreq: "weekly",
        priority: 0.7,
      });
    });

    smStream.end();
    streamToPromise(pipeline).then((sitemap) => res.send(sitemap));
  } catch (error) {
    console.error(error);
    res.status(500).end();
  }
});

const PORT = 3000;
app.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

### ✅ 이 방식의 장점:

- 데이터베이스에서 동적으로 페이지 URL을 가져올 수 있음.
- 새로운 게시글이 추가되면 자동으로 Sitemap이 업데이트됨.

---

## 4. `robots.txt`에 Sitemap 추가

Sitemap을 생성한 후, 검색 엔진이 이를 쉽게 찾을 수 있도록 `robots.txt` 파일에 추가하는 것이 좋습니다.

Express에서 `robots.txt`를 제공하는 간단한 방법:

```javascript
app.get("/robots.txt", (req, res) => {
  res.type("text/plain");
  res.send(
    "User-agent: *\nDisallow:\nSitemap: https://yourwebsite.com/sitemap.xml"
  );
});
```

---

## 5. Google Search Console에 Sitemap 제출

1. **Google Search Console**(https://search.google.com/search-console) 에 접속.
2. 사이트 추가 후 `https://yourwebsite.com/sitemap.xml`을 제출.

---

### 🔥 정리

- `sitemap` 패키지를 사용하면 Express.js에서 동적으로 Sitemap을 생성할 수 있음.
- `SitemapStream`을 사용하여 URL을 추가하고, `gzip`으로 압축 가능.
- 데이터베이스와 연동하여 동적인 페이지를 포함할 수 있음.
- `robots.txt`에 Sitemap URL을 추가하여 검색 엔진이 쉽게 찾도록 설정.
- Google Search Console에 제출하여 SEO 최적화 가능.

Express.js 기반 포트폴리오 사이트에도 적용할 수 있으니 한번 테스트해보면 좋을 것 같아! 🚀
