---
title: "AWS S3 + CloudFront 캐싱 최적화: 성능을 극대화하는 방법"
excerpt: "AWS S3 + CloudFront를 활용한 정적 웹사이트 배포 시 캐싱 최적화 방법을 알아봅니다. Cache-Control 설정, CloudFront 캐시 정책(Cache Policy) 설정, 그리고 캐시 무효화(Invalidation) 전략까지 자세히 설명합니다."
categories:
  - AWS
  - CloudFront
  - 웹 최적화
  - DevOps
  - 성능 최적화

tags:
  - [AWS, CloudFront, S3, 캐싱 최적화, 웹 성능, 배포 자동화]
permalink: /aws/cloudfront-caching-optimization/
toc: true
toc_sticky: true
date: 2025-02-13
last_modified_at: 2025-02-13
---

## AWS S3 + CloudFront 캐싱 최적화: 성능을 극대화하는 방법

AWS S3와 CloudFront를 활용해 정적 웹사이트를 배포할 때 **올바른 캐싱 전략**을 적용하면 성능을 극대화할 수 있습니다. 이 글에서는 **Cache-Control 설정, CloudFront Cache Policy 생성, 그리고 캐시 무효화(Invalidation) 전략**까지 정리합니다.

## 1️⃣ GitHub Actions에서 S3 업로드 시 `Cache-Control` 설정

GitHub Actions에서 `aws s3 sync`를 활용하여 S3에 업로드할 때 `Cache-Control`을 설정하면 브라우저가 파일을 적절하게 캐싱하도록 할 수 있습니다.

### 📌 **S3 파일별 캐싱 정책**

| 파일 유형                                                | Cache-Control 설정                       |
| -------------------------------------------------------- | ---------------------------------------- |
| `*.html`                                                 | `no-cache, no-store, must-revalidate`    |
| `*.css, *.js, *.png, *.jpg, *.svg`                       | `public, max-age=31536000, immutable`    |
| `robots.txt, sitemap.xml, site.webmanifest, favicon.ico` | `public, max-age=86400, must-revalidate` |

### **GitHub Actions 설정 예제**

```yaml
- name: Deploy HTML files (no-cache)
  run: |
    aws s3 sync public/ s3://www.javierju.com/ --delete \
      --exclude "*" --include "*.html" \
      --cache-control "no-cache, no-store, must-revalidate"

- name: Deploy static assets (long-term cache)
  run: |
    aws s3 sync public/ s3://www.javierju.com/ --delete \
      --exclude "*.html" --exclude "robots.txt" --exclude "sitemap.xml" --exclude "site.webmanifest" --exclude "favicon.ico" \
      --cache-control "public, max-age=31536000, immutable"

- name: Deploy SEO & Manifest files (1-day cache)
  run: |
    aws s3 sync public/ s3://www.javierju.com/ --delete \
      --exclude "*" --include "robots.txt" --include "sitemap.xml" --include "site.webmanifest" --include "favicon.ico" \
      --cache-control "public, max-age=86400, must-revalidate"
```

이렇게 하면 **HTML은 즉시 변경 반영**, 정적 파일(CSS, JS, 이미지)은 **장기 캐싱**, SEO 관련 파일은 **1일 캐싱**됩니다.

## 2️⃣ CloudFront 캐싱 정책(Cache Policy) 설정

GitHub Actions에서 `Cache-Control`을 설정했지만, **CloudFront는 자체적으로 캐싱 정책을 사용합니다**. 따라서 CloudFront에서 **파일별 캐싱 정책(Cache Policy)을 추가로 설정**해야 합니다.

### 📌 CloudFront에서 캐싱 정책 생성하기

#### **CloudFront에서 캐시 정책을 생성하는 방법**

```bash
aws cloudfront create-cache-policy --cache-policy-config '{
    "Name": "Custom-LongTermCache",
    "DefaultTTL": 31536000,
    "MaxTTL": 31536000,
    "MinTTL": 86400,
    "ParametersInCacheKeyAndForwardedToOrigin": {
        "EnableAcceptEncodingGzip": true,
        "CookiesConfig": {"CookieBehavior": "none"},
        "HeadersConfig": {"HeaderBehavior": "none"},
        "QueryStringsConfig": {"QueryStringBehavior": "all"}
    }
}'
```

### 📌 CloudFront Behavior 설정 변경

| Path Pattern                       | Origin Request Policy         | Cache Policy                |
| ---------------------------------- | ----------------------------- | --------------------------- |
| `*.html`                           | **AllViewerExceptHostHeader** | **Managed-CachingDisabled** |
| `*.css, *.js, *.png, *.jpg, *.svg` | **AllViewerExceptHostHeader** | **Custom-LongTermCache**    |

## 3️⃣ CloudFront 캐시 무효화(Invalidation) 자동화

새로운 파일을 배포할 때는 **CloudFront Invalidation을 사용하여 캐시를 초기화**해야 합니다.

```yaml
- name: Invalidate CloudFront Cache
  run: aws cloudfront create-invalidation --distribution-id E1GPXIPESV5ZA2 --paths "/*"
```

이렇게 하면 새로 배포된 HTML 파일이 **즉시 갱신**됩니다.

---

## 🚀 **최적화된 CloudFront 캐싱 전략 정리**

✔ **S3 파일 업로드 시 `Cache-Control` 적용 (GitHub Actions)**
✔ **CloudFront에서 `Cache Policy`를 설정하여 최적화된 캐싱 적용**
✔ **파일 업데이트 시 `Cache Invalidation`을 실행하여 최신 파일 반영**

이제 CloudFront와 S3를 활용한 최적의 캐싱 전략을 적용할 수 있습니다! 🚀
