---
title: "JavaScript HTTP 요청을 쉽게 관리하는 Axios 라이브러리 자세히 알아보기"
excerpt: "Axios는 Promise 기반의 HTTP 클라이언트로, 브라우저와 Node.js 환경에서 모두 사용할 수 있습니다. Axios의 기능과 사용법을 자세히 알아봅니다."
categories:
  - JavaScript
tags:
  - [JavaScript, Axios, HTTP, Web Development, Fetch API]
permalink: /JavaScript/axios-http-library/
toc: true
toc_sticky: true
date: 2024-12-5
last_modified_at: 2024-12-5
---

**Axios**는 JavaScript로 HTTP 요청을 보내고 응답을 처리하기 위해 만들어진 **Promise 기반의 HTTP 클라이언트 라이브러리**입니다. Axios는 브라우저 환경과 Node.js 환경에서 모두 사용할 수 있으며, 간결하고 직관적인 API를 제공하여 다양한 HTTP 요청을 쉽게 처리할 수 있습니다.

## Axios의 주요 기능

1. **Promise 기반**  
   Axios는 비동기 처리를 위한 Promise를 기본적으로 사용하며, `async/await` 구문과도 자연스럽게 호환됩니다.
   ```javascript
   axios.get('/api/data')
       .then(response => console.log(response))
       .catch(error => console.error(error));
   
   // 또는 async/await 사용
   async function fetchData() {
       try {
           const response = await axios.get('/api/data');
           console.log(response);
       } catch (error) {
           console.error(error);
       }
   }
   ```

2. **HTTP 메서드 지원**  
   Axios는 GET, POST, PUT, DELETE, PATCH, HEAD와 같은 HTTP 메서드를 지원합니다.

3. **요청 데이터 변환**  
   요청 데이터를 JSON으로 자동 변환하거나 원하는 형식으로 변환할 수 있습니다.

4. **응답 데이터 자동 변환**  
   서버에서 JSON 형태로 데이터를 반환하면 자동으로 JavaScript 객체로 변환합니다.

5. **요청 및 응답 헤더 설정**  
   요청 시에 필요한 헤더를 설정할 수 있고, 서버로부터의 응답 헤더를 확인할 수 있습니다.
   ```javascript
   axios.get('/api/data', {
       headers: {
           'Authorization': 'Bearer <token>',
           'Content-Type': 'application/json',
       },
   });
   ```

6. **인터셉터 (Interceptors)**  
   요청과 응답에 대한 전처리, 후처리를 할 수 있는 기능을 제공합니다.
   ```javascript
   axios.interceptors.request.use(config => {
       console.log('Request Sent:', config);
       return config;
   }, error => Promise.reject(error));

   axios.interceptors.response.use(response => {
       console.log('Response Received:', response);
       return response;
   }, error => Promise.reject(error));
   ```

7. **취소 토큰 지원**  
   진행 중인 요청을 취소할 수 있습니다.
   ```javascript
   const source = axios.CancelToken.source();
   
   axios.get('/api/data', { cancelToken: source.token })
       .catch(thrown => {
           if (axios.isCancel(thrown)) {
               console.log('Request canceled', thrown.message);
           }
       });

   source.cancel('Operation canceled by the user.');
   ```

8. **타임아웃 설정**  
   특정 시간 내에 응답이 없으면 요청을 중단할 수 있습니다.
   ```javascript
   axios.get('/api/data', { timeout: 5000 })
       .catch(error => {
           if (error.code === 'ECONNABORTED') {
               console.log('Request timeout');
           }
       });
   ```

9. **크로스 도메인 요청 지원**  
   CORS 문제를 해결하며, `withCredentials` 옵션으로 쿠키 전송을 처리할 수 있습니다.
   ```javascript
   axios.get('https://example.com/api/data', { withCredentials: true });
   ```

10. **Node.js와 브라우저에서 모두 작동**  
    서버와 클라이언트 양쪽에서 사용할 수 있어 서버사이드 렌더링(SSR) 프로젝트에서도 활용됩니다.

## Axios 설치 및 사용

### 설치
Axios는 npm 또는 CDN을 통해 설치할 수 있습니다.

1. **npm**  
   ```bash
   npm install axios
   ```

2. **CDN**
   ```html
   <script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
   ```

### 기본 사용법

1. **GET 요청**
   ```javascript
   axios.get('/api/data')
       .then(response => console.log(response.data))
       .catch(error => console.error(error));
   ```

2. **POST 요청**
   ```javascript
   axios.post('/api/data', {
       name: 'John',
       age: 30,
   })
       .then(response => console.log(response.data))
       .catch(error => console.error(error));
   ```

3. **기본 설정 생성**  
   Axios 인스턴스를 생성하여 공통 설정을 관리할 수 있습니다.
   ```javascript
   const apiClient = axios.create({
       baseURL: 'https://example.com/api',
       timeout: 1000,
       headers: { 'Authorization': 'Bearer <token>' },
   });

   apiClient.get('/data')
       .then(response => console.log(response.data))
       .catch(error => console.error(error));
   ```

## Axios와 Fetch API 비교

| 기능                  | Axios                                  | Fetch API                             |
|-----------------------|----------------------------------------|---------------------------------------|
| 브라우저 호환성       | IE 11 포함 대부분 브라우저 지원          | 최신 브라우저만 지원                   |
| 요청/응답 데이터 변환 | 자동 JSON 변환                         | 수동으로 JSON 변환 필요                 |
| 타임아웃 설정         | 기본 제공                             | 별도 구현 필요                         |
| 요청 취소             | 취소 토큰(CancelToken) 지원             | AbortController 필요                   |
| HTTP 인터셉터         | 기본 제공                             | 별도 구현 필요                         |
| API 사용의 직관성     | 더 간결하고 직관적                     | 더 많은 코드 작성 필요                  |

Axios는 간결한 문법, 강력한 기능, 다양한 옵션으로 인해 **React**, **Vue.js**, **Node.js** 프로젝트에서 널리 사용되고 있습니다. `fetch`보다 더 많은 기능과 간편한 사용성을 제공하기 때문에 복잡한 HTTP 요청 관리가 필요한 프로젝트에 특히 유용합니다.
