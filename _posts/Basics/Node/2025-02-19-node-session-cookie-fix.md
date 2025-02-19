---
title: "Express 세션 쿠키 문제 해결: Set-Cookie 미응답 오류와 Secure 설정"
excerpt: "Express에서 세션 쿠키가 브라우저에 설정되지 않는 문제를 해결하는 과정과 원인을 분석하고, 'secure' 설정을 'auto'로 변경하여 해결하는 방법을 코드 예제와 함께 설명합니다."
categories:
  - Node
  - Express
  - Backend
  - 오류 해결
  - 배포
  - PM2
  - Nginx

tags:
  - [Express, Node.js, Backend, 세션, 쿠키, 오류 해결, AWS, PM2, Nginx]
permalink: /node/session-cookie-fix/
toc: true
toc_sticky: true
date: 2025-02-19
last_modified_at: 2025-02-19
---

## 🔍 문제 상황

Express 기반 웹 애플리케이션을 AWS EC2 + PM2 + Nginx 환경에 배포한 후, **로그인 요청 시 `Set-Cookie`가 응답되지 않는 문제**가 발생했다.

### **증상**

1. **로그인은 정상적으로 동작**하지만, 브라우저에서 `Set-Cookie`가 응답되지 않음.
2. `pm2 logs`에서 `SESSION DATA AFTER LOGIN` 로그가 정상 출력됨.
3. `curl -i -X POST`로 확인해도 `Set-Cookie`가 응답되지 않음.
4. 로컬 개발 환경에서는 정상적으로 동작.

🚨 **즉, Express는 세션을 생성했지만 `Set-Cookie`가 응답되지 않는 문제!**

---

## 🔎 원인 분석

### **1. `secure: true` 설정 문제**

기본적으로 `secure: true`는 **HTTPS 환경에서만 쿠키를 설정하도록 강제**한다.

- 로컬에서는 HTTP이므로 문제없이 동작했지만,
- AWS 배포 환경에서는 Nginx가 HTTPS 트래픽을 받고 내부에서는 HTTP로 요청을 전달하기 때문에, Express는 "보안되지 않은 HTTP 요청"으로 간주하고 `Set-Cookie`를 설정하지 않았음.

### **2. `trust proxy` 설정 문제**

AWS 환경에서는 `app.set('trust proxy', 1);` 설정이 필요하다.
이 설정이 없으면 Express는 **프록시 뒤에서 온 요청을 신뢰하지 않아 `secure: true`가 동작하지 않음.**

### **3. PM2 실행 환경 문제**

PM2를 사용하면 환경 변수가 제대로 반영되지 않을 수 있음. `--update-env` 옵션이 필요함.

---

## 🚀 해결 방법

### ✅ **1. `secure: true` → `secure: 'auto'`로 변경**

🔹 `middlewares/session.js` 수정

```javascript
const sessionConfig = (dbUrl, secret) => {
  const store = MongoStore.create({
    mongoUrl: dbUrl,
    collectionName: "sessions",
    stringify: false,
    autoRemove: "interval",
    autoRemoveInterval: 10,
    touchAfter: 24 * 60 * 60,
    crypto: { secret },
  });

  store.on("error", (e) => {
    console.log("SESSION STORE ERROR", e);
  });

  return session({
    store,
    name: "session",
    secret,
    resave: false,
    saveUninitialized: false,
    proxy: true,
    cookie: {
      httpOnly: true,
      secure: process.env.NODE_ENV === "production" ? "auto" : false, // ✅ auto로 변경
      sameSite: "Lax",
      maxAge: 1000 * 60 * 60 * 24 * 7, // 1주일
    },
  });
};

module.exports = sessionConfig;
```

이렇게 변경하면:

- HTTPS 환경에서는 `secure: true`로 자동 설정됨.
- HTTP 환경에서는 `secure: false`로 설정되어 `Set-Cookie`가 정상적으로 동작.

---

### ✅ **2. `trust proxy` 설정 확인 (`app.js`)**

```javascript
app.set("trust proxy", 1);
```

이 설정이 없으면, Express는 요청이 HTTP라고 판단하여 `secure: true` 쿠키를 설정하지 않음.

---

### ✅ **3. PM2 환경 변수 업데이트 후 재시작**

```sh
pm2 restart all --update-env
```

이렇게 하지 않으면 PM2가 환경 변수를 캐싱하여 `NODE_ENV=production`이 반영되지 않을 수 있음.

---

### ✅ **4. HTTPS 환경에서 `Set-Cookie` 응답 확인**

```sh
curl -i -X POST https://www.yelp-camp.com/login -c cookies.txt
```

- HTTPS 환경에서 `Set-Cookie`가 응답되는지 확인.

---

## 🎯 **결론**

🔹 `secure: true`를 사용하면 **HTTPS가 아닌 요청에서는 쿠키가 설정되지 않음.**
🔹 `secure: 'auto'`로 변경하면 **환경에 따라 자동 조정되어 문제 해결.**
🔹 `trust proxy` 설정이 없으면 Express가 HTTPS 요청을 신뢰하지 않음.
🔹 PM2 실행 시 `--update-env` 옵션을 사용해야 환경 변수가 반영됨.

✅ **위 설정을 적용한 후 `Set-Cookie`가 정상적으로 응답되었고, 로그인 상태 유지가 가능해짐!** 🎉

---

💡 **비슷한 문제를 겪는다면 위 해결 방법을 적용해보고, HTTPS 환경과 프록시 설정을 꼭 확인하자!** 🚀
