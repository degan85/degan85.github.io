---
layout: post
title: "Vue Components basic"
author: Degan
categories: vue
tags:	vue component components
comments: true
---

#### 컴포넌트란?
- 조합하여 화면을 구성할 수 있는 블록
- 화면을 빠르게 구조화하여 일괄적인 패턴으로 개발 가능

#### 컴포넌트 영역

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=component-scope.html"></script>

- 결과

```
첫 번째 지역 컴포넌트 : 100
두 번째 지역 컴포넌트 :
```

#### 지역 컴포넌트와 전역 컴포넌트의 차이

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=global-local-components.html"></script>

- 결과

```
첫 번째 인스턴스 영역

전역 컴포넌트입니다.

지역 컴포넌트입니다.

----------

두 번째 인스턴스 영역

전역 컴포넌트입니다.
```

#### 뷰 컴포넌트 통신

- 앵귤러1이나 백본(Backbone.js)
  - 초창기 자바스크립트 프레임워크
  - 한 화면을 1개의 뷰(view)로 간주
  - 한 화면의 데이터를 해당 화면 어디서든지 호출 가능
- 뷰(vue.js)
  - 컴포넌트로 화면을 구성
  - 같은 웹 페이지라도 데이터를 공유할 수 없음
  - 각 컴포넌트의 유효 범위가 독립적이기 때문
- 상,하위 컴포넌트 관계
  - 상위에서 하위로는 props라는 특별한 속성을 전달
  - 하위에서 상위로는 기본적으로 이벤트만 전달
    - 이벤트와 함께 데이터를 전달하고 싶은 경우
    - 이벤트의 두 번째 인자 값으로 잔달 하거나
    - 이벤트 버스(Event Bus)를 활용
![상하관계](https://kr.vuejs.org/images/props-events.png)
    - 출처: https://kr.vuejs.org

- 상위에서 하위로 props 전달

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=components-props.html"></script>


- 하위에서 상위로 이벤트 전달

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=components-event.html"></script>

- 이벤트 버스

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=eventbus.html"></script>

---

## Reference


[Do it! Vue.js 입문](http://www.yes24.com/24/goods/58206961)
