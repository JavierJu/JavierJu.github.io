---
title: "SDK란 무엇인가? Mapbox SDK 예시로 이해하기"
excerpt: "SDK는 소프트웨어 개발을 위한 도구 모음으로, Mapbox와 같은 서비스에서 제공하는 SDK가 개발자에게 어떻게 도움이 되는지 설명합니다."
categories:
  - 개발
tags:
  - [SDK, Mapbox, 개발자 도구, JavaScript, iOS, Android]
permalink: /info/dev-sdk-introduction/
toc: true
toc_sticky: true
date: 2025-01-09
last_modified_at: 2025-01-09
---

## SDK란 무엇인가?

**SDK**는 **Software Development Kit**의 약자로, 소프트웨어 개발을 위한 도구 모음입니다. 개발자는 SDK를 사용하여 특정 플랫폼, 서비스, 또는 하드웨어에서 애플리케이션을 개발하거나 통합할 수 있습니다.

SDK는 주로 다음과 같은 구성 요소를 포함합니다:

- **라이브러리 및 API**: 특정 기능을 쉽게 사용할 수 있도록 제공되는 코드 모음.
- **도구**: 디버깅, 테스트, 애플리케이션 개발을 지원하는 유틸리티.
- **문서 및 샘플 코드**: 개발자가 SDK를 효과적으로 사용할 수 있도록 돕는 가이드와 예제 코드.
- **에뮬레이터**: 실제 환경을 시뮬레이션하여 테스트할 수 있는 도구.

### SDK 예시: Mapbox SDK

Mapbox는 위치 기반 서비스를 위한 다양한 SDK를 제공합니다. 이를 통해 개발자는 웹, iOS, Android 애플리케이션에 지도를 표시하거나 내비게이션 기능을 추가할 수 있습니다.

#### 1. **Mapbox GL JS**

**Mapbox GL JS**는 JavaScript 기반 라이브러리로, 웹 애플리케이션에서 인터랙티브한 지도를 렌더링하고 다양한 기능을 추가할 수 있도록 도와줍니다.

```javascript
// Mapbox GL JS 예시
mapboxgl.accessToken = 'YOUR_ACCESS_TOKEN';
var map = new mapboxgl.Map({
  container: 'map', // HTML 요소 ID
  style: 'mapbox://styles/mapbox/streets-v11', // 스타일 URL
  center: [0, 0], // 중심 좌표 (경도, 위도)
  zoom: 2 // 초기 줌 레벨
});
```

위 코드는 Mapbox GL JS를 사용하여 웹 페이지에 기본적인 지도를 표시하는 예시입니다.

#### 2. **Mapbox Navigation SDK**

Mapbox Navigation SDK는 iOS 및 Android 애플리케이션에서 내비게이션 기능을 추가하는 데 사용됩니다. 예를 들어, 길 찾기, 교통 정보, 경로 안내 등을 제공할 수 있습니다.

```swift
// iOS에서 Mapbox Navigation SDK 예시
import MapboxNavigation

let navigationService = MapboxNavigationService(accessToken: "YOUR_ACCESS_TOKEN")
let navigationViewController = NavigationViewController(for: route, navigationService: navigationService)
navigationViewController.modalPresentationStyle = .fullScreen
present(navigationViewController, animated: true, completion: nil)
```

이 코드는 iOS 애플리케이션에서 Mapbox Navigation SDK를 사용하여 내비게이션 기능을 구현하는 예시입니다.

### SDK와 API의 차이

SDK와 **API**는 비슷한 개념처럼 보일 수 있지만, 그 목적과 범위에서 차이가 있습니다:

- **SDK**는 애플리케이션 개발에 필요한 도구와 라이브러리를 종합적으로 제공하는 도구 세트입니다.
- **API**는 특정 기능에 접근할 수 있는 인터페이스로, SDK에 포함될 수 있지만 독립적으로도 사용됩니다.

즉, SDK는 개발을 돕는 '도구 상자'와 같고, API는 그 안에서 사용하는 '도구'라고 할 수 있습니다.

### 마무리

SDK는 개발자가 애플리케이션을 보다 효율적이고 강력하게 만들 수 있도록 돕는 필수적인 도구입니다. Mapbox와 같은 서비스에서 제공하는 SDK는 지리 정보, 내비게이션 기능 등 복잡한 기능을 쉽게 구현할 수 있게 해줍니다.
