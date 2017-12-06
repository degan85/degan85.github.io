---
layout: post
title:  "HTTP 프록시와 버프스위트"
author: Degan
categories: white-hat
tags:	http proxy bupsuite
comments: true
---

## 목차
1. [HTTP 프록시](#http_proxy)
2. [Burp Suite 설정](#burp_suite_setting)
3. [브라우저 설정](#browser_setting)
4. [프록시 intercept](#proxy_intercept)
5. [요청, 응답 메시지](#req_res_mesg)
6. [버프스위트 그외 기능들](#burp_suite_etc)
2. [실습 후](#after_proxy)

---

<a name="http_proxy"></a>
## 1. HTTP 프록시
- HTTP 요청과 응답을 중간에 가로채는 것이 가능
- 메시지 내용을 볼 수 있고, 수정 가능
- 버프스위트(Burp Suite) 애플리케이션 사용

---

<a name="burp_suite_setting"></a>
## 2. Burp Suite 설정
- 버프스위트 실행 후 Proxy -> Option 탭을 누르면 interface에 ip주소, 포트번호 나옴(127.0.0.1:8080)
- Runing 체크가 되있으면 실행되고 있는 것임.
	
---

<a name="browser_setting"></a>
## 3. 브라우저 설정
- 브라우저 오른쪽에 Preferences를 통해 설정
![preferences](https://degan85.github.io/assets/proxy-burp-suite-1.png)

- Advanced -> Network -> Settings
![settings](https://degan85.github.io/assets/proxy-burp-suite-2.png)

- Manual proxy configuration를 체크 
- HTTP Proxy에 버프스위트 interface에 있던 ip와 포트번호 입력
- No Proxy for란은 지움
![settings](https://degan85.github.io/assets/proxy-burp-suite-3.png)

---

<a name="proxy_intercept"></a>
## 4. 프록시 intercept
- 버프스위트 Proxy탭 선택
- intercept 탭 선택
- `intercept is on` 확인(껐다 켰다 가능)
- intercept하면 메시지가 잡히고 서버로는 아직 전송되지 않음
- HTTP는 타임아웃 개념이 없어서 장시간 intercept해도 상관없음
- `intercept is off`를 하면 프록시를 통해 전송되기는 하지만 화면에 내용을 표시하지 않음
- 브라우저에서 **localhost/dvwa/** 접속
- intercept된 화면
![intercept](https://degan85.github.io/assets/proxy-burp-suite-4.png)

- intercept돼있는 동안 브라우저는 화면이 표시되지 않음
- 버프스위트에서 `Forward`를 누르면 그제서야 브라우저 화면이 나옴
![connected_dvwa](https://degan85.github.io/assets/proxy-burp-suite-5.png)

- `Drop`을 누르면 아애 메시지를 차단함

---

<a name="req_res_mesg"></a>
## 5. 요청, 응답 메시지
### 요청 메시지
```
GET /dvwa/ HTTP/1.1
Host: localhost
User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:52.0) Gecko/20100101 Firefox/52.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Cookie: security=impossible; PHPSESSID=mmt6kmraqnfq76ssn4svold7s6
Connection: close
Upgrade-Insecure-Requests: 1
```
- GET : HTTP 메소드
- /dvwa/ : url
- Host : web site 주소
- User-Agent, Accept, Accept-Language, Accept-Encoding ... : HTTP header(각종 정보)
- Cookie : 	PHPSESSID 중요(하이재킹 공격 가능)

### 응답 메시지
```
HTTP/1.1 302 Found
Date: Sun, 03 Dec 2017 14:44:52 GMT
Server: Apache/2.4.26 (Unix) OpenSSL/1.0.2l PHP/5.6.31 mod_perl/2.0.8-dev Perl/v5.16.3
X-Powered-By: PHP/5.6.31
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate, post-check=0, pre-check=0
Pragma: no-cache
Location: login.php
Content-Length: 0
Connection: close
Content-Type: text/html; charset=UTF-8
```
- 302 : Redirect 의미
- Location : Redirect 되는 위치
- 나머지 header 정보를 통해 서버 상태를 파악 가능
- 보안을 목적으로 해당 정보들을 수정해서 보내기도 함


<a name="burp_suite_etc"></a>
## 6. 버프스위트 그외 기능들
![site_map](https://degan85.github.io/assets/proxy-burp-suite-6.png)
- `Target`
	* 접속한 호스트들과 url들을 트리로 보여줌
	* 선명한 글씨는 직접 방문한 곳을 나타냄 
	* 흐린 글씨는 버프스위트가 http메시지를 분석해서 포함된 링크들을 자동으로 보여줌
	* 위에 DVWA 접속 페이지를 보면 파란색으로 링크가 있음
	* 그 링크(hiderefer.com/?http://www.dvwa.co.uk/)가 두번째 흐린 글씨(hiderefer.com)로 표시 됨
- `Proxy`
	* 'HTTP history` : 접속했던 host와 url, 요청과 응답 메시지 확인 가능
- `Scanner` : 보안 취약점을 분석해서 보여줌(유료)
- `Intruder` : 요청 메시지의 일부분을 지정해서 여러개의 데이터를 반복해서 전송(퍼징)
- `Repeater` : 수동으로 test할때 유용
	* `HTTP history`에서 test하고 싶은 요청을 마우스 오른쪽 클릭
	* `Send to Repeater`를 선택
	* 해당 요청이 `Repeater` 탭에 등록
	* `Go` 버튼을 누르면 응답의 요청을 바로 확인
	* 요청 메시지를 수정해서 보내도 바로 응답 메시지 옴(test하기 좋음)
	
---

<a name="after_proxy"></a>
## 7. 실습 후
- 브라우저를 이용할때 계속 정지 할 수 있음
- 프록시 설정을 초기화 해줘야함	
![after_proxy](https://degan85.github.io/assets/proxy-burp-suite-7.png)

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
