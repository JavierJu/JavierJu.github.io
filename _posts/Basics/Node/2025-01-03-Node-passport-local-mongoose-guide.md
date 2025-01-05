---
title: "passport-local-mongoose: Node.js 인증을 간단하게"
excerpt: "passport-local-mongoose를 활용해 로컬 인증을 구현하는 방법과 주요 기능을 자세히 알아봅니다."
categories:
  - Node
  - Authentication
tags:
  - [JavaScript, Node.js, Passport.js, Mongoose, Backend]
permalink: /node/passport-local-mongoose-guide/
toc: true
toc_sticky: true
date: 2025-01-03
last_modified_at: 2025-01-03
---

## passport-local-mongoose란?

`passport-local-mongoose`는 [Passport.js](http://www.passportjs.org/)와 함께 사용되는 Mongoose 플러그인으로, 로컬 인증(local authentication)을 구현하는 데 유용한 도구입니다. 이 플러그인은 사용자의 이메일이나 사용자 이름(username)과 비밀번호를 기반으로 인증 기능을 손쉽게 설정할 수 있도록 여러 가지 편리한 기능을 제공합니다.

### 주요 특징

1. **패스워드 해싱 및 검증**:
   - 비밀번호를 평문으로 저장하지 않고, 해시를 생성하여 안전하게 저장합니다.
   - 사용자가 로그인 시 입력한 비밀번호를 해시와 비교하여 인증을 처리합니다.

2. **편리한 API**:
   - 사용자 등록(register), 인증(authenticate), 로그인(login), 로그아웃(logout) 등을 처리하는 간단한 메서드를 제공합니다.

3. **Mongoose와의 통합**:
   - MongoDB와 Mongoose 모델을 기반으로 작동하며, 기존 Mongoose 스키마에 쉽게 플러그인을 추가할 수 있습니다.
   - 인증과 관련된 필드(`username`, `hash`, `salt`)를 자동으로 추가합니다.

4. **구성 옵션**:
   - 사용자 정의 필드나 옵션을 쉽게 설정할 수 있습니다. 예를 들어, 사용자 이름 대신 이메일을 사용할 수 있도록 구성할 수 있습니다.
   - 암호화 알고리즘이나 해시 길이 등도 설정 가능합니다.

---

## 설치 및 기본 사용법

### 1. 설치
```bash
npm install passport passport-local-mongoose mongoose
```

### 2. 사용자 모델 정의
Mongoose와 `passport-local-mongoose`를 사용하여 사용자 모델을 정의합니다.

```javascript
const mongoose = require('mongoose');
const passportLocalMongoose = require('passport-local-mongoose');

const UserSchema = new mongoose.Schema({
  email: { type: String, required: true, unique: true }
});

// passport-local-mongoose를 스키마에 플러그인으로 추가
UserSchema.plugin(passportLocalMongoose, { usernameField: 'email' });

const User = mongoose.model('User', UserSchema);
module.exports = User;
```

`usernameField` 옵션을 설정하면 사용자 이름 대신 이메일을 사용자 이름 필드로 사용할 수 있습니다.

---

### 3. Passport 설정
Passport와 `passport-local-mongoose`를 함께 사용하는 설정입니다.

```javascript
const passport = require('passport');
const LocalStrategy = require('passport-local');
const User = require('./models/User');

// Passport 설정
passport.use(new LocalStrategy(User.authenticate()));
passport.serializeUser(User.serializeUser());
passport.deserializeUser(User.deserializeUser());
```

---

### 4. 사용자 등록 및 인증
다음은 Express를 사용하여 사용자 등록 및 로그인 기능을 구현한 예입니다.

```javascript
const express = require('express');
const mongoose = require('mongoose');
const session = require('express-session');
const passport = require('passport');
const User = require('./models/User');

const app = express();

mongoose.connect('mongodb://localhost/test', { useNewUrlParser: true, useUnifiedTopology: true });

app.use(express.urlencoded({ extended: true }));
app.use(session({
  secret: 'yourSecret',
  resave: false,
  saveUninitialized: false
}));
app.use(passport.initialize());
app.use(passport.session());

// 사용자 등록 라우트
app.post('/register', async (req, res) => {
  try {
    const user = await User.register(new User({ email: req.body.email }), req.body.password);
    res.send('User registered successfully');
  } catch (err) {
    res.send(err.message);
  }
});

// 사용자 로그인 라우트
app.post('/login', passport.authenticate('local', {
  successRedirect: '/dashboard',
  failureRedirect: '/login'
}));

// 대시보드 (인증된 사용자만 접근 가능)
app.get('/dashboard', (req, res) => {
  if (req.isAuthenticated()) {
    res.send('Welcome to your dashboard');
  } else {
    res.redirect('/login');
  }
});

app.listen(3000, () => console.log('Server running on http://localhost:3000'));
```

---

## 주요 메서드

1. `User.register(user, password)`  
   - 새로운 사용자를 등록하고, 비밀번호를 해싱하여 저장합니다.

2. `User.authenticate()`  
   - 인증 전략에서 사용되는 메서드로, 입력한 비밀번호와 저장된 비밀번호를 비교합니다.

3. `User.serializeUser()` 및 `User.deserializeUser()`  
   - 세션에서 사용자 정보를 저장하거나 복원하는 데 사용됩니다.

---

## 커스터마이징

- **비밀번호 규칙 강화**  
  Passport 자체는 비밀번호 규칙 검증을 제공하지 않으므로, 사용자 등록 전에 비밀번호 유효성을 추가로 검증하는 로직을 구현할 수 있습니다.
  
- **JWT와의 통합**  
  `passport-local-mongoose`는 세션 기반 인증을 기본으로 사용하지만, JWT(JSON Web Tokens)와 함께 사용할 수도 있습니다.

---

`passport-local-mongoose`는 단순히 로컬 인증을 처리하는 데 초점을 맞추고 있어, 확장 가능성과 사용 편의성을 동시에 제공합니다. 필요에 따라 Mongoose 스키마를 확장하거나 Passport의 다양한 인증 전략과 결합하여 더 복잡한 인증 시스템을 구축할 수 있습니다.

