---
title: "MongoDB 연결 시 인증 문제 해결: ?authSource=admin 사용법"
excerpt: "MongoDB 연결 중 발생하는 Authentication failed 오류를 해결하기 위해 ?authSource=admin 옵션을 추가하는 방법과 그 원리를 알아봅니다."
categories:
  - MongoDB
tags:
  - [MongoDB, Node.js, Backend, 인증 문제]
permalink: /mongodb/mongodb-auth-source/
toc: true
toc_sticky: true
date: 2024-12-19
last_modified_at: 2024-12-19
---

MongoDB를 사용하여 데이터를 관리할 때, `Authentication failed` 오류를 경험한 적이 있나요? 특히 인증 관련 설정이 명확하지 않을 경우 이 문제가 발생할 수 있습니다. 아래에서는 `?authSource=admin` 옵션을 추가하여 문제를 해결하는 과정을 공유합니다.

## 문제 상황

AWS EC2에 MongoDB를 설치한 후, Node.js 애플리케이션에서 다음 코드를 통해 MongoDB에 연결하려고 했습니다.

```javascript
const mongoose = require('mongoose');

mongoose.connect('mongodb://username:password@hostIP:27017/farmStand')
    .then(() => {
        console.log("MONGO CONNECTION OPEN");
    })
    .catch(err => {
        console.log('OH NO MONGO CONNECTION ERROR!');
        console.log(err);
    });
```

그러나 실행 시 `MongoServerError: Authentication failed` 오류가 발생했습니다.

## 원인 분석

MongoDB는 사용자가 인증을 수행해야 하는 데이터베이스를 기본적으로 현재 연결 중인 데이터베이스로 간주합니다. 위 코드에서 데이터베이스 이름 `farmStand`는 아직 생성되지 않았기 때문에, 인증 오류가 발생한 것입니다.

## 해결 방법

MongoDB의 인증 데이터베이스를 명시적으로 설정하기 위해 URI에 `?authSource=admin` 옵션을 추가했습니다. 수정된 코드는 다음과 같습니다:

```javascript
mongoose.connect('mongodb://username:password@hostIP:27017/farmStand?authSource=admin')
    .then(() => {
        console.log("MONGO CONNECTION OPEN");
    })
    .catch(err => {
        console.log('OH NO MONGO CONNECTION ERROR!');
        console.log(err);
    });
```

## 옵션 설명

- `authSource=admin`: MongoDB가 사용자 인증을 `admin` 데이터베이스에서 수행하도록 지정합니다.

## 결과

`?authSource=admin`를 추가한 후, 연결이 성공적으로 이루어졌습니다. MongoDB 인증 문제를 해결하기 위해 이와 같은 설정을 사용하는 것이 유용합니다.

## 결론

MongoDB를 사용할 때 인증 데이터베이스를 명확히 설정하는 것이 중요합니다. 특히 `admin` 데이터베이스를 사용하여 사용자 인증을 수행할 경우, URI에 `?authSource=admin`을 추가하는 것을 잊지 마세요. 이를 통해 인증 관련 오류를 손쉽게 해결할 수 있습니다.

도움이 되셨길 바랍니다! 😊

