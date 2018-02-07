---
layout: post
title: " Eclipse에 Android 개발환경 구축 "
author: Degan
categories: problem
tags:	eclipse android andmore sdk
comments: true
---

## 발생
- 안드로이드 스튜디오를 보통 사용하지만 
- 업무상 Eclipse에 Android 환경 구축해야 함
- 구글에서는 더이상 ADT 플러그인에 대한 지원하지 않음
- SDK 설치 후 `dx.jar` error
- **Device Target Unknown** 문제 

## 해결
- Reference에 블로그 참고[^1]
- ADT 플러그인 대신 이클립스 재단의 Andmore 플러그인 사용
	- **[Help]** > **[Eclipse Marketplace]**
	- **adt** 검색 -> **Andmore:Development Tools...** 설치
	- 재시작 후 **install Android ...** 체크
	- 자신의 경로에 android-sdks가 설치 됨
- Eclipse에서 **[Window]** > **[Perspective]** > **[Customize Perspective]**
	- 좌측에 Android로 시작하는 것 4개 체크 
	- SDK Manager 생성 됨
- SDK Manager 실행
	- Build-tools에서 API 27, 25 설치
	- `dx.jar` Error발생하면  25.0.3/lib 안에 `dx.jar`를 27에 덮어씌움
- 휴대폰에 맞는 USB Driver 설치
- **Device Target Unknown** 문제 발생하면
	- 휴대폰 재부팅 후 다시 연결
	- 또는 adb 죽이고 다시 시작

---

## Reference

[^1]:[Eclipse + Andmore를 이용한 이클립스에서 안드로이드 구동하기](http://zzdd1558.tistory.com/180)
