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
![settings](https://lh3.googleusercontent.com/4qOdkUdwnkt-uver5cbBPxZniKYyoW_AkTceW9wBO_MHrHOcSm6sXiE-7hoCobHredzXEvF_2t16Zwq8nPDaLAxnNntNZyjHbacLfNHXNCFZVjljBQtYSurIZxYBrGUIJd545r_kazYywAW8nI0-lDvr9lzdpkiSaHiJY7Xn1qU1eH31INtrPUOdyKt8nmtz5DA29BdJlm2TBhUdfDkb68pY6P8pB__tweHCBjf6XuWeXFkZBscIQhqhNL_lwihCnlM-44c-Eb375e9CnZYe-0iAHHWIJRIRV5dvEriX3jIALNL7eznEO7075MCulAdM_TiW4sZ-anvHVkIloiBClL5XloiZhRLp-fcZWw-TLaZ9YNFfr3c0qVl-dtaTl3SrnxlCVplS0LY5giDENwQ-Gxtf-8ClpUPtPkACrYs2FT1Y2kTINioD5FgRstphaV-GXlrKJtTOUBqwoX_yfjNUuUYvdROY95LTgzIblDyyEnKqENw2f5SNtsuOGZny1ORwH2aydmPizHKxmjZes5zFyy24iHJ29aCauAWf3h-3gh2y5mRnOL5eon_LsNsXXbnJpK2mklvRuqj6aieehoaaEKdL_2dYWjgX2h3IWNxniA=w944-h934-no)

- Manual proxy configuration를 체크 
- HTTP Proxy에 버프스위트 interface에 있던 ip와 포트번호 입력
- No Proxy for란은 지움
![settings](https://lh3.googleusercontent.com/kELZWckPlp3eprXclVIjtqeAlgFcXvrC_wSh9qDwWXKh1V8ppW_cowp6he_S07rrIL4DrOtOuenzXbyL7gC_1B-BSd6nEGI5FOMahIWw7RH6zEiWn6v7oqUC8FZMMA1E0l6pxWFWr05bONBFApIqvc8kSgNyZ2oSJzqs2fNY8XC2hykVJESigpR7BsECZ-qGMooiWGo9lB3C-6_bKr7qTF5-W2ph-bj3owAp7EgSY9RPZ4wTs6Hp_N6qPSjwVg35dKZh7V0Ze2cl_tTB38DiIdyOWzHDsgECQ8_IpnB2kkh4Qw8CcuG739rR3Pfx7DLSN_1H4Ptt52nAySBSqxJ7jBfxwUWXC3Ncpqrq6TPx6Ve7iLJK3MgEBm3kldTG7bSqi7DbZCnWslChIb7xzHNjXYhS7c3w9RtJgeJQ_v-kQVUCErJ6_tg17OCpte6DL3sQoF4_Ep8bktAmXbOwbl3RLbTIFIvA0KDgCabhmMMHVRKr7vyurRHXwHmOhLNsCUCOaGoNhkhKek_WBsOlRtlc1DB8IsbCcwLCcRw0breOSSxM7pXwcfBjSFsX3o6B1gTE5NgHTOx56TuECNVOOeRxsD68H2IA5vHpSqU7lrHD1A=w944-h814-no)

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
![intercept](https://lh3.googleusercontent.com/oCORAXjawEQNvQzArwpileNIWcE9ih5dbFuLwJclIHOjC2S_5sJOjnPAwjPgm4vJMt9lrCBVqGm6b68G0h7v7uu-oVbLgOEmw7QsBkXGY8W0e_gvW4cnEfj-8EGf48nCVd3U50CtMGA8N30ucIhNQyqSmzBpvwL65olx5zuhKIdlzzy8E_dvxNxeFg-XyBtKYskvucaR3fOQRoqVdI3a0IHLEU-HmtuoidtQD_sBTDaLmwDbK980xIXDqZ2HSNPoN5fl_Q0CWUblMdFWR2zCYmGM2Ee0J-ia1tuhlhlxzWIy4TVir-Uc0gs_SillZFH54d-Sr3NxbNJIyVvjAgw3voZD_gqP2aNwKMJk64QTB5objL_WQufgUOKlgKZ2rKUrCP7LJKufrYxx-5D2ZZhR0Ra7iPrWCSXe52N1_yKeaym7656jwiSkQ4ETjPfSkQ483FAyDa_F5uxpfXznlAzq3ejn2ZJElBXiTFBTvYL_jj4Q7RKNBEQke2dPIb7pgkH0wJw9vhKt89WpJFXtAFPQ0CazCHKT0h49u83GH49b_dDV7JG6xPqLFvHf0EEP4GuG9ZuIMTyGdxD1Qd3ntzZaMNOxoJGmTZPcuFNU_rpgrg=w944-h873-no)

- intercept돼있는 동안 브라우저는 화면이 표시되지 않음
- 버프스위트에서 `Forward`를 누르면 그제서야 브라우저 화면이 나옴
![connected_dvwa](https://lh3.googleusercontent.com/GCnIPl_DrDdJ4k8L8Y2Tn1RuyatRA9YWD05qNh6JY-vyMigkT3a8hQnHHHZMDgFhqAQ2BknImv7QrWJ2IxUZUhE83p1nI-BFp1WAakZX_gd9XphY0CnIuVtiZSdsV7U6gUmMMTdJaHQ9Ulfo9dROgP80qu1JRslV3MjTif9mR6BX8jkixZpY2sNZpwGWJ3zFngOIAm5vriaE4SZiYNMSFP_NISthXYWxzZkUW2asThN_V3okUow5KRiLaTkUw6ttblk7chPWPP29LebKPfcutUq6C9gmBOHo7zV4Oa04gDaWpjuqOY3u9QNgrp1ofrY7x4J0jnnF5Ur8teNCeFE6_XyGEhd3gwe8iwcZBUVVaym2TvAydIRRJSWz5UqNpfWiDVzgyCPaN5dGTi0AWOJ2SoszGL1-CuyH6POUH39Af4OW2xUnJKY8owO0Fr00ktW90FsPtFmBgxSabruDAoBszjmKPhwB_58peAwmblOKJ__Q81Lm4QgEySVA2oSE0ZEQo2ishZPQ6fXo1fRRvTedT7BzKogtzu2tQos8kCSiUFYMo0rRUySpomv5MIwID1eyxDipOkHMwPnqfh9fczBTN2hhpS3A1bwFyJNkkvWo6Q=w944-h893-no)

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
![site_map](https://lh3.googleusercontent.com/F6w6C8uT9pah8viozcNBpdZ2fNsOT-eggsr1_1mgf-ttLmZKjDNoVeVyvEjEABM0Au9fYT7Sc9TJXxSf08TPG0BDtP9Ch5ro9whiQF6C0eTWSbNBB4mDdC4DTk4E-esZ0LqpfXAv6kEwVO0s5XF0TG97V7EtktP9IDwHwK7H8YnVa5LdSXH49ufJm3FXKkvAKufICw6exNWEkSYu2pupSm5vW6_TZJVV6-1qSHOA-SYRpdjCc1SO66cxb87rwHL4b_86xPtYXNuwhSubCdxRTqQATt7qEthS3tHI5Bo6aA8PjVrKDt71ZWkzHQELPTeYO6BlrCk9Jvrg5Gdn3zziwbHSzaaQdKEkpZeE7ICnQPwc8Gv08vbEAZtbt8q5lJG4ia-8fMAJdN67SG7Dw1-mSiGqdmnkGw0kNxD5h7VUp1vI5quAwJoxjoifi0M943isx3nQgk9oCNG_aHYFKBhNzT_Umhq4ob0qsAhX8J2Oxpr0GEzMeKn0nBYk7PfBLSLuAF0nSB7A5Ccu5_McEmCd7D1XLlOLn8ejch2xjBB0jm3dV9XNsxEiSzKesSThB8pRh1MJC-FtZ0D9B12jKHEeTmZrH7TFJIcY_7AwdVf86g=w944-h543-no)
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
![after_proxy](https://lh3.googleusercontent.com/6xv8HX4oZiB2BPryqHqyaco65YJ_u-UgMuBrH-dM1kKDBkepvTEaz_-j9Si7v66bY22XEX5MK_zWNiuwFj6-rJAYxZTGIDYSmBXGHHO5nipLxmeI6Ye8IZZvT0TaTh_2J7-5vu9l0KolO-6OHGeNQKjRLHlrihY7S2_CpJwu1DIbI8OfAEbq5_mX1hu8Ac9cSR3q4LrgYOSjcdMD7nFnvSXFGqLoO3aJCVZWvtc09d_Ar_HX7TfW_ejUnG_p7oXj0uNqiCT1ZrwukRXBqkf8tBcWPNBY9aEMYXxHH2bwc9YLxHBq5xxIIV0Ju-ILhnElDpnSo2Zypz14t3JDVU8XEU24y12lboGK5tDLRHuiz6VWlnEzgxmd6oCTyubS7URrM1Rrmw2yAudlpWkWVR1N2cHUIy--KnKurIZOS_OaLPGcYiPkNCkOEFOPOr_q5vCDd44RyULfLMV4GNqxgoD_WXQO35D3QHbz4jvTeGyVgcakT2ufE3-s3Mthj5nPlbG-5a2WI2Ek2XfbeEDdSDBtnzkhSqnyQneJgyfeEpCpy-eE7HtzpYBey9E6hVnNlMmc1-0AIPLSTBW0yYhMSrNeb2mUsZSCGLjLgkRQEEFnKg=w944-h860-no)

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
