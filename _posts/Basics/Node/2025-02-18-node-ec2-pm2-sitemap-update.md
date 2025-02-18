---
title: "EC2 + PM2 환경에서 Sitemap 자동 생성 및 업데이트 설정"
excerpt: "EC2 서버에서 PM2를 사용하여 배포된 Node.js 앱에서 Sitemap을 자동 생성하고 최신 상태로 유지하는 방법을 설명합니다. PM2 재시작 시, 크론 잡, 동적 URL 반영을 위한 설정 방법을 코드와 함께 제공합니다."
categories:
  - Node
  - Express
  - SEO
  - PM2
  - DevOps
tags:
  - [Node.js, Express, SEO, AWS, PM2, DevOps, Sitemap]
permalink: /node/ec2-pm2-sitemap-update/
toc: true
toc_sticky: true
date: 2025-02-18
last_modified_at: 2025-02-18
---

# EC2 + PM2 환경에서 Sitemap 자동 생성 및 업데이트 설정

Node.js 기반의 Express 애플리케이션을 EC2 서버에서 PM2로 실행할 때, Sitemap을 자동으로 생성하고 최신 상태로 유지하는 방법을 정리합니다. 특히 **동적인 URL (예: 개별 캠프그라운드 페이지)까지 포함하는 Sitemap을 효율적으로 업데이트하는 방법**을 다룹니다.

## ✅ 현재 동작 방식 (PM2 재시작 시 Sitemap 생성)

현재 `server.js`에서 `generateSitemap()`을 실행하도록 설정하면 **PM2가 재시작될 때마다 Sitemap이 새로 생성**됩니다.

📄 **`server.js`**

```javascript
const express = require("express");
const path = require("path");
const generateSitemap = require("./scripts/sitemap");

const app = express();

// 서버 실행 시 Sitemap 생성
generateSitemap()
  .then(() => {
    console.log("✅ Sitemap updated on server start");
  })
  .catch((err) => {
    console.error("❌ Sitemap generation failed:", err);
  });

// 정적 파일 제공
app.use(
  "/sitemap.xml",
  express.static(path.join(__dirname, "public", "sitemap.xml"))
);

app.listen(3000, () => {
  console.log("🚀 Server running at http://localhost:3000");
});
```

✅ **동작 방식:**

- `pm2 restart app` 실행 시 `server.js`가 다시 실행되면서 Sitemap이 업데이트됨.

❌ **문제점:**

- 새로운 캠프그라운드가 추가되어도 **서버가 재시작되지 않으면 Sitemap이 갱신되지 않음**.

## ✅ Sitemap을 자동 업데이트하는 3가지 방법

### **방법 1: PM2 재시작 시 생성 (현재 방식)**

위에서 설명한 대로 **PM2를 재시작할 때만 Sitemap이 갱신됨**. 하지만 실시간 업데이트가 어려움.

### **방법 2: `cron`을 이용해 주기적으로 Sitemap 생성 (추천)**

PM2 재시작 없이 **매일 또는 일정 간격으로 자동 생성**하려면 `cron`을 사용하면 됨.

📌 **EC2에서 `crontab -e` 실행 후 추가**

```
0 * * * * cd /home/ubuntu/YelpCamp && npm run generate-sitemap
```

(위 설정은 매 **정각마다 (`0 * * * *`)** `npm run generate-sitemap` 실행)

✅ **장점:** 항상 최신 상태 유지됨, 서버 재시작 필요 없음.

### **방법 3: 캠프그라운드가 추가될 때 자동으로 Sitemap 생성**

새로운 캠프그라운드가 추가될 때 즉시 Sitemap을 갱신하도록 `routes/campgrounds.js`에서 `generateSitemap()`을 실행함.

📄 **`routes/campgrounds.js`**

```javascript
const express = require("express");
const router = express.Router();
const Campground = require("../models/campground");
const generateSitemap = require("../scripts/sitemap");

// 새로운 캠프그라운드 추가
router.post("/", async (req, res) => {
  try {
    const newCampground = new Campground(req.body);
    await newCampground.save();

    // 캠프그라운드 추가 후 Sitemap 갱신
    await generateSitemap();
    console.log("✅ Sitemap updated after new campground addition");

    res.redirect(`/campgrounds/${newCampground._id}`);
  } catch (error) {
    console.error("❌ Error adding campground:", error);
    res.status(500).send("Internal Server Error");
  }
});

module.exports = router;
```

✅ **장점:**

- 새 캠프그라운드가 추가될 때마다 즉시 Sitemap 업데이트됨.

❌ **단점:**

- 트래픽이 많을 경우 DB 부하가 발생할 가능성이 있음.

## 🚀 최종 추천 설정

| 방법                                         | PM2 재시작 필요 | 자동 반영 속도    | 서버 부하 |
| -------------------------------------------- | --------------- | ----------------- | --------- |
| **방법 1: PM2 재시작 시 생성**               | O               | 느림              | 낮음      |
| **방법 2: `cron`을 이용한 자동 생성 (추천)** | X               | 빠름 (최대 1시간) | 낮음      |
| **방법 3: 새 캠프그라운드 추가 시 생성**     | X               | 즉시 반영         | 다소 부담 |

### **🚀 추천 조합**

✔ `cron`을 이용해 매일 혹은 매시간 Sitemap을 자동 생성 (`방법 2`)
✔ 추가로 PM2 재시작 시 (`pm2 restart`) Sitemap을 갱신 (`방법 1`)
✔ 캠프그라운드가 자주 추가된다면 `방법 3`을 적용하여 즉시 반영

이제 **EC2 환경에서도 최신 Sitemap을 자동으로 유지할 수 있음! 🚀🔥**
