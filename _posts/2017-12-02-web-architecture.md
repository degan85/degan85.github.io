---
layout: post
title:  "웹 아키텍쳐"
author: Degan
categories: white-hat 
tags:	white_hat hacker web architecture
comments: true
---

- 클라이언트 -> 인터넷 -> 서버(로직 티어, 데이터 티어)

## HTTP
 - Hyper Text Transfer Protocol
 - 주로 80번 포트를 이용해서 서비스
 - 요청(클라이언트)과 응답(서버)으로 이루어짐
 - HTTP 메소드 : GET, POST, PUT 등
 - 응답코드(status code) : 요청의 처리 여부
	- 1xx : 정보전달
	- 2xx : 성공
	- 3xx : 리다이렉션
	- 4xx : 클라이언트 쪽 에러
	- 5xx : 서버 에러
	
 - HTTP는 stateless이기 때문에 session ID를 사용해서 이전 상태를 유지
 - session ID는 보통 쿠키로 전달
	- 쿠키
		- key : value 집합
		- 서버는 Set-Cookie 헤더를 이용해 쿠키를 전달
		- 브라우저에서는 이를 저장했다가 Cookie 헤더에 전송
		- session ID를 탈취함으로서 세션 하이재킹 공격 가능
	
---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
