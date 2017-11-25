---
layout: post
title:  "Chrome developer tools"
date:   2017-08-29
author: Degan
categories: javascript
tags:	javascript chrome project
comments: true
---

### SOURCE 

`source`탭 `XHR Breakpoint`에서 오른쪽에 `+` 누르고 Enter하면 모든 Ajax통신 시 breakpoint가 걸립니다.

`Watch` 누르고 `+`를 눌러 변수명을 입력하면 해당 변수를 주시할 수 있습니다.


### NETWORK

`network`탭을 이용해서 HTTP 통신과정 볼 수 있습니다.

 하단에 `DOMContentLoaded`(파란색, 파란선)과 Load(빨간색, 빨간선)으로 로딩 시간을 확인할 수 있습니다.(Load의 경우 이미지까지 불러온 시간)

`녹화버튼` 오른쪽에 카메라 아이콘을 누르면 녹화모드가 작동합니다.
ms단위로 화면이 만들어지는 과정이 스냅샷으로 제공됩니다.(ctrl+R)

`filter`(카메라 옆) 아이콘을 누르고 response와 request 헤더값의 정보를 필터해서 볼 수 있습니다.(ex. ajax는 XHR 누름)

---

## Reference
[programmers 자바스크립트와-웹-프런트엔드](https://www.welcomekakao.com/learn/courses/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EC%99%80-%EC%9B%B9-%ED%94%84%EB%A1%A0%ED%8A%B8%EC%97%94%EB%93%9C)
