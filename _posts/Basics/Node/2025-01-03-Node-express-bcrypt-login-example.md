---
title: "Express와 Bcrypt를 이용한 간단한 로그인 구현"
excerpt: "Express와 Bcrypt를 활용한 사용자 등록, 로그인, 세션 관리 방법을 살펴보고, 안전한 비밀번호 저장과 인증 방식을 구현합니다."
categories:
  - Node
  - Express
  - Authentication
tags:
  - [Node.js, Express, Bcrypt, Authentication, Backend]
permalink: /node/express-bcrypt-login-example/
toc: true
toc_sticky: true
date: 2025-01-03 
last_modified_at: 2025-01-03 
---

## 소개

이 글에서는 Node.js 기반의 Express 프레임워크와 Bcrypt 라이브러리를 사용해 간단한 로그인 시스템을 구현하는 방법을 알아봅니다. MongoDB를 활용한 사용자 데이터 저장, 비밀번호 해싱, 세션 관리를 단계별로 설명합니다. 코드를 통해 실제 사용 예시를 확인할 수 있습니다.

---

## 주요 구현 사항

### 1. 사용자 모델 정의 (`user.js`)

사용자 정보를 저장하고 인증하는 데 필요한 스키마와 메서드를 정의합니다.

- **비밀번호 해싱**: 사용자가 입력한 비밀번호를 저장하기 전에 `bcrypt.hash`를 사용해 해싱 처리합니다. 이는 데이터 유출 시에도 비밀번호가 안전하도록 보호합니다.
- **비밀번호 검증**: 사용자가 입력한 비밀번호와 저장된 해시 값을 `bcrypt.compare`로 비교합니다.
- **`pre` 훅 사용**: 비밀번호가 변경될 때만 해싱 처리를 수행합니다.

#### 코드:

```javascript
userSchema.statics.findAndValidate = async function (username, password) {
    const foundUser = await this.findOne({ username });
    const isValid = await bcrypt.compare(password, foundUser.password);
    return isValid ? foundUser : false;
}

userSchema.pre('save', async function (next) {
    if (!this.isModified('password')) return next();
    this.password = await bcrypt.hash(this.password, 12);
    next();
})
```

---

### 2. 회원가입 처리 (`index.js`)

사용자가 회원가입 폼을 통해 입력한 데이터를 저장합니다.

- **새 사용자 생성**: 사용자가 입력한 `username`과 `password`를 기반으로 사용자 데이터를 생성합니다.
- **세션 저장**: 새로 생성된 사용자의 ID를 세션에 저장하여 로그인 상태를 유지합니다.

#### 코드:

```javascript
app.get('/register', (req, res) => {
    res.render('register')
})

app.post('/register', async (req, res) => {
    const { password, username } = req.body;
    const user = new User({ username, password });
    await user.save();
    req.session.user_id = user._id;
    res.redirect('/');
})
```

---

### 3. 로그인 처리 (`index.js`)

사용자가 입력한 정보를 검증하고 인증 상태를 유지합니다.

- **사용자 검증**: `User.findAndValidate` 메서드를 호출해 사용자 이름과 비밀번호를 확인합니다.
- **로그인 성공 시 세션 저장**: 검증에 성공하면 세션에 사용자 ID를 저장합니다.
- **로그인 실패 처리**: 검증 실패 시 로그인 페이지로 리다이렉션합니다.

#### 코드:

```javascript
app.get('/login', (req, res) => {
    res.render('login')
})

app.post('/login', async (req, res) => {
    const { username, password } = req.body;
    const foundUser = await User.findAndValidate(username, password);
    if (foundUser) {
        req.session.user_id = foundUser._id;
        res.redirect('/secret');
    } else {
        res.redirect('/login');
    }
})
```

---

### 4. 비밀 페이지 보호 (`index.js`)

인증된 사용자만 비밀 페이지에 접근할 수 있도록 미들웨어를 사용합니다.

- **미들웨어 `requireLogin`**: 세션에 사용자 ID가 없으면 로그인 페이지로 리다이렉션합니다.

#### 코드:

```javascript
const requireLogin = (req, res, next) => {
    if (!req.session.user_id) {
        return res.redirect('/login');
    }
    next();
}

app.get('/secret', requireLogin, (req, res) => {
    res.render('secret');
})
```

---

### 5. 로그아웃 처리 (`index.js`)

사용자가 로그아웃하면 세션에서 사용자 ID를 제거합니다.

#### 코드:

```javascript
app.post('/logout', (req, res) => {
    req.session.user_id = null;
    res.redirect('/login');
})
```

---

## 개선 가능 사항

1. **비밀번호 복잡성 검사**
   - 사용자가 더 안전한 비밀번호를 설정할 수 있도록, 회원가입 시 비밀번호 복잡성을 검증하는 기능을 추가할 수 있습니다.
   - 예를 들어, 최소 길이, 대문자, 숫자, 특수 문자의 포함 여부를 확인할 수 있습니다.

2. **CSRF 보호**
   - CSRF(Cross-Site Request Forgery) 공격을 방지하기 위해, CSRF 토큰을 활용한 보안 기능을 구현할 수 있습니다.

3. **HTTPS 적용**
   - 네트워크 상에서 데이터를 암호화하기 위해 HTTPS를 사용하는 것이 좋습니다. 이를 위해 SSL 인증서를 적용합니다.

4. **계정 잠금 기능**
   - 반복적인 로그인 실패 시 계정을 일시적으로 잠그는 기능을 추가하여 무차별 대입 공격을 방지할 수 있습니다.

5. **세션 만료 시간 설정**
   - 세션의 유효 기간을 설정하여, 일정 시간 동안 활동이 없으면 자동으로 로그아웃되도록 구현할 수 있습니다.

6. **이메일 인증**
   - 회원가입 후 이메일 인증 절차를 추가하면, 가짜 계정을 줄이고 보안을 강화할 수 있습니다.

---

## 전체 코드

### **`user.js`**

```javascript
const mongoose = require('mongoose');
const bcrypt = require('bcrypt')

const userSchema = new mongoose.Schema({
    username: {
        type: String,
        required: [true, 'Username cannot be blank']
    },
    password: {
        type: String,
        required: [true, 'Password cannot be blank']
    }
})

userSchema.statics.findAndValidate = async function (username, password) {
    const foundUser = await this.findOne({ username });
    const isValid = await bcrypt.compare(password, foundUser.password);
    return isValid ? foundUser : false;
}

userSchema.pre('save', async function (next) {
    if (!this.isModified('password')) return next();
    this.password = await bcrypt.hash(this.password, 12);
    next();
})

module.exports = mongoose.model('User', userSchema);
```

### **`index.js`**

```javascript
const express = require('express');
const app = express();
const User = require('./models/user');
const mongoose = require('mongoose');
const session = require('express-session');

mongoose.connect('mongodb://127.0.0.1:27017/loginDemo')
    .then(() => {
        console.log("MONGO CONNECTION OPEN!!!")
    })
    .catch(err => {
        console.log("OH NO MONGO CONNECTION ERROR!!!!")
        console.log(err)
    })

app.set('view engine', 'ejs');
app.set('views', 'views');

app.use(express.urlencoded({ extended: true }));
app.use(session({ secret: 'notagoodsecret' }))

const requireLogin = (req, res, next) => {
    if (!req.session.user_id) {
        return res.redirect('/login');
    }
    next();
}

app.get('/', (req, res) => {
    res.send('THIS IS THE HOME PAGE');
})

app.get('/register', (req, res) => {
    res.render('register');
})

app.post('/register', async (req, res) => {
    const { password, username } = req.body;
    const user = new User({ username, password });
    await user.save();
    req.session.user_id = user._id;
    res.redirect('/');
})

app.get('/login', (req, res) => {
    res.render('login');
})

app.post('/login', async (req, res) => {
    const { username, password } = req.body;
    const foundUser = await User.findAndValidate(username, password);
    if (foundUser) {
        req.session.user_id = foundUser._id;
        res.redirect('/secret');
    } else {
        res.redirect('/login');
    }
})

app.post('/logout', (req, res) => {
    req.session.user_id = null;
    res.redirect('/login');
})

app.get('/secret', requireLogin, (req, res) => {
    res.render('secret');
})

app.listen(3000, () => {
    console.log("SERVING YOUR APP!");
})
```

