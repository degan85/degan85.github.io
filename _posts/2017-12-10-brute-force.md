---
layout: post
title:  "Brute force 공격"
author: Degan
categories: white-hat 
tags:	white_hat hacker dvwa brute force
comments: true
---

- 사용자 패스워드 알아내기 위한 공격
- 하나하나 패스워드 계속 대입
	- 알파벳 순
	- 딕셔너리 공격(자주쓰는 단어 이용)

- 브라우저에 버프스위트 interface 설정
- 퍼프스위트 프록시 `intercept is off`
([HTTP 프록시와 버프스위트](https://degan85.github.io/white-hat/2017/12/06/proxy-burp-suite.html) 참고) 
- DVWA에서 로그인 시도
![brute-force](https://degan85.github.io/assets/brute-force-0.png)

- password가 틀렸다고 나옴
![brute-force](https://degan85.github.io/assets/brute-force-00.png)

- 버프스위트 `Proxy` -> `HTTP history` -> 로그인 응답 마우스 오른쪽 클릭 -> `send to Intruder`
![brute-force](https://degan85.github.io/assets/brute-force-1.png)

- 공격할 변수들이 자동 선택됨
- 비밀번호만 공격할 것이므로 `Clear`를 누르고 내가 입력했던 비밀번호(wwwww)만 선택해서 `Add` 클릭
![brute-force](https://degan85.github.io/assets/brute-force-2.png)

- 'Payload' 탭 클릭

### 알파벳 순
- `payload type`을 'Brute force`선택
- `Character set`에 다양하게 넣을 문자열 입력
- `Min length`에 최소 입력 길이 , `Max length`에 최대 입력 길이를 써 넣음
	- 특수 문자 없을 때 min, max =4 일때 :  1,679,616개의 경우의 수
	- 특수문자(*&) 추가, max를 5로 변경하면 81,320,304의 경우의 수
	- 비밀번호 생성할 때 길이를 6이상 특수 문자, 숫자를 포함해야 하는 이유를 알 수 있음
- `Start attack` 실행
- 앞에 a부터 하나씩 공격. 언젠가는 성공
	- aaaa -> baaa -> caaa ...
![brute-force](https://degan85.github.io/assets/brute-force-3.png)


### 딕셔너리
- 통계적으로 많이 사용하는 단어들의 리스트를 사용
- 아래 경로에서 **password.lst**사용. 또는 인터넷에서 list를 받아서 사용
![brute-force](https://degan85.github.io/assets/brute-force-4.png)

- `payload type`에 'Simple list`선택
- `load`를 누르고 list를 불러옴
- 'Start attack` 실행
![brute-force](https://degan85.github.io/assets/brute-force-5.png)

- 'Length` (응답 길이) 값이 다른 것이 사용자가 사용하는 password
- 아래의 예에서는 **password**라는 값이 `5352`로 응답 길이가 다름
![brute-force](https://degan85.github.io/assets/brute-force-6.png)

- 해당 문자열을 비밀번호에 입력
- 로그인 성공
![brute-force](https://degan85.github.io/assets/brute-force-7.png)

### 공격 대응 방법
- level을 medium으로 올려서 확인
	- 잘못된 password를 입력했더니 응답이 조금 느리게 옴
	- 응답 소스에 `sleep( 2 )`를 줘서 공격 속도를 늦추게 됨
![brute-force](https://degan85.github.io/assets/brute-force-8.png)

- level을 high로 올림
	- 응답 소스에 `sleep( rand(0,3) )` 줌
	- 해커가 일정한 시간이 지연 되면 실패인 것을 알 수 있으므로 실패시 랜덤하게 응답 시간을 보냄
![brute-force](https://degan85.github.io/assets/brute-force-9.png)

- 3번 틀릴 경우 15분 대기
	- 거의 블루트 포스 방법으로는 해킹이 불가능
	- 해커가 일부러 패스워드를 틀려서 사용자가 접속 못하게 할 수 있음
![brute-force](https://degan85.github.io/assets/brute-force-10.png)

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
