---
title: "PWA에서 '앱 설치'와 '홈 화면 추가' 차이점과 manifest.json 설정 방법"
excerpt: "홈 화면에 웹 앱을 설치할 때 '앱 설치'로 변경되는 이유와 manifest.json 설정을 통해 앱 이름을 바꾸는 방법에 대해 설명합니다."
categories:
  - Tools
tags:
  - [Github Blog, Web Development, PWA, Manifest, Web App]
permalink: /Tools/pwa-app-install-home-screen/
toc: true
toc_sticky: true
date: 2024-11-25
last_modified_at: 2024-11-25
---

웹 앱을 홈 화면에 추가하는 기능이 **PWA (Progressive Web App)**로 확장되면서, 예전의 '바로가기 만들기'에서 '앱 설치'로 변경된 이유와 그 과정에 대해 설명합니다. 또한, `manifest.json` 파일에서 앱 이름을 변경하는 방법도 알아봅니다.

### 1. '앱 설치'로 변경된 이유

브라우저가 웹 앱을 **PWA**로 인식할 경우, 사용자가 홈 화면에 추가할 때 **"앱 설치"**로 표시되는 현상이 발생합니다. 이는 웹 앱 매니페스트(`manifest.json`) 파일에서 앱의 설정을 통해 **PWA로 인식되기 때문**입니다. 이전에는 단순한 바로가기 형태로 추가되었지만, PWA 설정을 통해 웹 앱이 **앱처럼 동작**하게 됩니다.

PWA를 설치하면 웹 앱이 **홈 화면에 아이콘 형태로 추가**되고, **앱처럼 실행**됩니다. 이는 오프라인에서도 작동할 수 있도록 도와주며, 앱에 가까운 사용자 경험을 제공합니다.

### 2. manifest.json 파일을 통한 앱 이름 설정

홈 화면에 추가된 웹 앱의 이름은 `manifest.json` 파일에서 설정할 수 있습니다. 이 파일의 `"name"` 항목을 수정하면, 홈 화면에 표시되는 앱 이름을 변경할 수 있습니다.

예를 들어, 기본적으로 `"name": "App"`으로 되어 있다면 이를 변경하여 원하는 이름을 설정할 수 있습니다.

#### 예시:
```json
{
  "name": "My Custom App Name",
  "icons": [
    {
      "src": "/images/favicon/android-icon-36x36.png",
      "sizes": "36x36",
      "type": "image/png",
      "density": "0.75"
    },
    {
      "src": "/images/favicon/android-icon-48x48.png",
      "sizes": "48x48",
      "type": "image/png",
      "density": "1.0"
    },
    {
      "src": "/images/favicon/android-icon-72x72.png",
      "sizes": "72x72",
      "type": "image/png",
      "density": "1.5"
    },
    {
      "src": "/images/favicon/android-icon-96x96.png",
      "sizes": "96x96",
      "type": "image/png",
      "density": "2.0"
    },
    {
      "src": "/images/favicon/android-icon-144x144.png",
      "sizes": "144x144",
      "type": "image/png",
      "density": "3.0"
    },
    {
      "src": "/images/favicon/android-icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png",
      "density": "4.0"
    }
  ]
}
```
위의 예시에서 `"name": "My Custom App Name"`으로 변경하면, 앱 이름이 홈 화면에 추가될 때 해당 이름으로 표시됩니다.

### 3. PWA로 인식되기 위한 조건

PWA로 인식되기 위해서는 **웹 앱 매니페스트** 외에도 **서비스 워커**를 설정해야 합니다. 서비스 워커는 웹 페이지가 오프라인일 때도 정상적으로 작동할 수 있도록 도와주는 중요한 역할을 합니다. 만약 서비스 워커 설정이 없다면, PWA로 인식되지 않으며 일반 웹 앱처럼 동작하게 됩니다.

#### 서비스 워커 추가 예시:
```js
if ('serviceWorker' in navigator) {
  navigator.serviceWorker.register('/service-worker.js').then(function(registration) {
    console.log('Service Worker registered with scope:', registration.scope);
  }).catch(function(error) {
    console.log('Service Worker registration failed:', error);
  });
}
```

### 결론

PWA는 **앱 설치**와 **홈 화면 추가**를 통한 **앱 같은 사용자 경험**을 제공합니다. `manifest.json`을 통해 앱 이름을 설정할 수 있으며, 서비스 워커를 추가하여 오프라인에서도 사용할 수 있는 기능을 구현할 수 있습니다. 이러한 설정을 통해 웹 앱을 더욱 향상된 형태로 제공할 수 있습니다.
