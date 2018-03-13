---
layout: post
title: "Vue Instance basic"
author: Degan
categories: vue
tags:	vue instance
comments: true
---

#### 뷰 인스턴스의 정의와 속성
- 뷰로 화면을 개발하기 위해 필수로 생성해야 하는 기본 단위

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=hello-vue.html"></script>

#### 적용 과정
1. 뷰 라이브러리 파일 로딩
2. 인스턴스 객체 생성
3. 특정 화면 요소에 인스턴스를 붙임
4. 인스턴스 내용이 화면 요소로 변환
5. 변환된 화면 요소를 사용자가 최종 확인

#### 뷰 인스턴스 라이프 사이클

- beforeCreate
	- 가장 처음으로 실행
	- data 속성과 method 속성이 아직 정의되어 있지 않음
	- 돔과 같은 화면 요소에 접근할 수 없음
-  created
	- data 속성과 method 속성이 정의되었기 때문에 속성에 정의된 값에 접근 가능
	- 서버에서 데이터를 요청하여 받아오는 로직을 수행하기 좋음
- beforeMount
	- template 속성에 지정한 마크업 속성을 render() 함수로 변환한 후 el 속성에 지정한 DOM에 인스턴스를 부착하기 전에 호출 단계
	- render() 함수가 호출되기 직전의 로직을 추가하기 좋음
- mounted
	- el 속성에서 지정한 화면 요소에 인스턴스가 부착되고 나면 호출
	- template 속성에 정의한 DOM에 접근할 수 있어서 화면 요소를 제어하는 로직을 수행하기 좋음
	- 변환 시점이 다를 경우 $nextTick() API를 활용
- beforeUpdate
	- 인스턴스가 부착되고 나면 정의한 속성들이 화면에 치환됨
	-  치환된 값은 뷰의 반응성을 제공하기 위해 $watch 속성으로 감시(데이터 관찰)
	- 관찰하고 있는 데이터가 변경되면 가상 돔으로 화면을 다시 그리기 전에 호출되는 단계
	- 변경 예정인 새 데이터에 접근, 관련 로직을 미리 넣을 수 있음
- updated
	- 데이터가 변경되고 나서 가상 돔으로 다시 화면을 그리고 나면 실행되는 단계
	- 데이터 변경 후 화면 요소 제어와 관련된 로직을 추가하기 좋은 단계
	- 데이터 값을 갱신하는 로직은 가급적 beforeUpdate에 추가하고, updated에서는 변경 데이터의 DOM 관련 로직을 추가하는 것이 좋음
- beforeDestroy
	- 뷰 인스턴스가 파괴되기 직전에 호출
	- 뷰 인스턴스의 데이터를 삭제하기 좋은 단계
- destroyed
	- 뷰 인스턴스가 파괴되고 나서 호출

---

## Reference


[Do it! Vue.js 입문](http://www.yes24.com/24/goods/58206961)
