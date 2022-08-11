---
layout: post
title:  모바일 APP 개발
date:   2022-08-11
author: Degan
categories: app
tags: mobile app react-native flutter native 
comments: true
---

## 개발 방법
- native
- 하이브리드
- 크로스 플랫폼

### native
- 특정 하드웨어나 OS에서 그대로 실행 됨
  - android
    - java or kotlin 언어 사용
    - android studio 개발 툴 사용
  - ios
    - object-C or swift 언어 사용
    - Xcode 개발 툴 사용 
- 운영체제에서 제공하는 다양한 api를 모두 사용 가능
- 개발, 운영에 공수가 많이 소모 됨(중복 개발)

### 하이브리드
- 초반에 웹뷰를 어플리케이션으로 묶어서 사용
  - native API를 사용할 수 없음
- CORDOVA 사용
  - native api 사용 가능
- 여전히 웹뷰라서 한계가 많음
- CORDOVA 엔진을 브릿지로 사용해서 메모리 사용, 속도에 단점

### 크로스 플랫폼
- 두 운영체제에서 동작하는 하나의 코드
- React Native (facebook)
  - Javascript 언어 사용
  - 모바일 api를 이용해서 모바일 UI 업데이트
  - JS code + Engine(모바일 api로 번역) => APK
  - 실시간으로 JS code를 변환해서 모바일 api로 업데이트
- FLUTTER (google)
  - Dart 언어 사용
  - FLUTTER Engine(C++) 자체적으로 canvas를 이용해서 UI 업데이트
- 운영체제에서 업데이트가 발생 할 경우 크로스 플랫폼에서 지원해 줄 때까지 기다려야 함
- facebook과 google에 의존해야 함
  - 지원하지 않으면 사라짐

## FLUTTER VS React Native
### [트랜드](https://trends.google.com/trends/explore?cat=31&date=today%205-y&q=FLUTTER,%2Fg%2F11h03gfxy9)
<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/3045_RC01/embed_loader.js"></script> <script type="text/javascript"> trends.embed.renderExploreWidget("TIMESERIES", {"comparisonItem":[{"keyword":"FLUTTER","geo":"","time":"today 5-y"},{"keyword":"/g/11h03gfxy9","geo":"","time":"today 5-y"}],"category":31,"property":""}, {"exploreQuery":"cat=31&date=today%205-y&q=FLUTTER,%2Fg%2F11h03gfxy9","guestPath":"https://trends.google.com:443/trends/embed/"}); </script>
- 초반에는 React Native가 우세
  - React Native가 먼저 나와서 커뮤니티가 안정적이였음
- 2020년말부터 FLUTTER의 관심도가 역전하기 시작 
- 현재는 flutter가 크게 높음 (2022/08/11 기준)

### 개발
- learning curve
  - 언어는 javascript를 사용하는 React Native가 익숙함
  - FLUTTER는 Dart를 새로 배워야함
- UI
  - React Native는 bridge를 이용해서 그려서 때에따라 분기가 필요
    - OS에 따라 다를 수 있음
  - FLUTTER는 C++로 작성된 그래픽엔진(Skia Engine)으로 자체적으로 canvas에 그림
    - 플랫폼에 의존적이지 않음

---

## Reference

[모바일 앱 개발의 현재와 미래](https://youtu.be/2AS0WAOX8_8)

[모바일 크로스플랫폼 Flutter vs React Native 차이가 뭘까?](https://velog.io/@sysout-achieve/%EB%AA%A8%EB%B0%94%EC%9D%BC-%ED%81%AC%EB%A1%9C%EC%8A%A4%ED%94%8C%EB%9E%AB%ED%8F%BCFlutter-vs-React-Native-%EC%B0%A8%EC%9D%B4%EA%B0%80-%EB%AD%98%EA%B9%8C)
