---
title: "[GIT Blog] 블로그에 댓글 기능 추가하기"
excerpt: "Disqus, Utterances, Facebook Comments Plugin을 사용하여 간단히 블로그에 댓글 기능을 추가하는 방법을 알아보세요. 외부 댓글 서비스와 직접 구현하는 방법을 소개합니다."

categories:
  - GIT
tags:
  - [Comments, Git Blog, Disqus, Utterances, Facebook Comments, 블로그 댓글]

permalink: /GIT/comments/

toc: true
toc_sticky: true

date: 2024-10-28
last_modified_at: 2024-10-28
---
GitHub 블로그에 댓글 기능을 추가하고자 외부 서비스와 직접 구현 방식을 비교하고 Utterances, Disqus 등 옵션별 특징에 대해 ChatGPT와 문답한 내용을 정리하였다. 현재 이 블로그는 Utterances를 적용하고 있다.

# 블로그에 댓글 기능 추가하기

블로그에 댓글 기능을 추가하고 싶다면, 외부 서비스를 이용하거나 직접 구현하는 두 가지 방법이 있습니다. 각각의 방법을 소개합니다.

## 1. 외부 댓글 서비스 사용

가장 간단한 방법은 외부 댓글 서비스(예: Disqus, Utterances, Facebook Comments Plugin)를 사용하는 것입니다.

### Disqus
널리 사용되는 댓글 시스템으로, 간단히 설치할 수 있고 스팸 방지 기능을 제공합니다.

- **사용 방법**: Disqus에 가입하고, 제공된 스크립트를 블로그에 추가합니다.
- **관리**: 댓글 관리는 Disqus 사이트에서 진행합니다.

### Utterances
GitHub Issues를 기반으로 한 댓글 시스템입니다. GitHub Pages나 정적 블로그에 잘 어울립니다.

- **사용 방법**: GitHub 레포지토리를 설정하고, 스크립트를 추가합니다.

```html
<script src="https://utteranc.es/client.js"
        repo="GitHubID/레포지토리명"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>
```
- **특징**: GitHub Issues를 통해 댓글을 관리하므로, 오픈소스 프로젝트 관련 블로그에 적합합니다.

### Facebook Comments Plugin
Facebook 계정을 이용해 댓글을 남길 수 있는 시스템으로, 사용자 인증이 간편합니다.

## 2. 직접 댓글 기능 구현

외부 서비스를 사용하지 않고 직접 구현할 수도 있습니다. 이 경우 필요한 요소는 다음과 같습니다.

- **백엔드 서버**: PHP, Node.js 등을 이용해 댓글 데이터를 처리하고 저장하는 서버를 구축합니다.
- **데이터베이스**: MySQL, MongoDB 등을 이용해 댓글 데이터를 저장하고 불러옵니다.
- **프론트엔드**: HTML/CSS와 JavaScript로 댓글 작성 폼과 목록 UI를 구현합니다. AJAX 요청으로 댓글을 비동기 추가할 수도 있습니다.

## 3. Jekyll 블로그에 Utterances 사용

GitHub Pages 기반의 Jekyll 블로그라면 Utterances를 사용하는 방법이 간단하고 적합합니다. GitHub 레포지토리와 바로 연동되어 백엔드 설정이 필요하지 않습니다.

## 추가 팁

- **스팸 방지**: reCAPTCHA 같은 스팸 방지 기능을 고려할 수 있습니다.
- **댓글 관리 기능**: 필요에 따라 댓글을 관리하는 페이지나 기능을 추가할 수 있습니다.

댓글 기능을 추가하여 독자와의 소통을 더 편리하게 만드세요!
