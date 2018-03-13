---
layout: post
title: "Vue Router"
author: Degan
categories: vue
tags:	vue router routing namedview nestedrouter
comments: true
---

####라이팅이란?
- 웹 페이지 간의 이동 방법
- SPA(Single Page Application)에서 주로 사용
- 화면 간의 전환이 매끄러움
- 애플리케이션의 사용자 경험을 향상시켜 줌
  - 깜빡거림 없이 화면을 매끄럽게 전환
  - 더 빠르게 화면을 조작할 수 있음
- 리액트, 앵귤러 모두 라우팅을 이용하여 화면을 전환
- HTML 파일들도 라우팅 자바스크립트 라이브러리를 이용하면 구현 가능

#### 뷰 라우터(Vue Router)
- 뷰에서 라우팅 기능을 구현할 수 있도록 지원하는 공식 라이브러리
- `<router-link to="URL 값">`
  - 페이지 이동 태그
  - 화면에는 `<a>`로 표시
  - 클릭하면 to에 지정한 URL로 이동
- '<router-view>'
  - 페이지 표시 태그
  - 변경되는 URL에 따라 해당 컴포넌트를 뿌려주는 영역

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=router.html"></script>

#### 네스티드 라우터(Nested Router)
- URL에 따라서 컴포넌트의 하위 컴포넌트가 다르게 표시
- 컴포넌트의 수가 적을 때는 유용
- 한 번에 더 많은 컴포넌트를 표시하는 데는 한계가 있음

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=nested-router.html"></script>

#### 네임드 뷰(Named View)
- 특정 페이지로 이동했을 때 여러개의 컴포넌트를 동시에 표시
- 네스티드 라우터는 상위 컴포넌트가 하위를 포함하는 형식
- 네임드 뷰는 같은 레벨에서 여러개의 컴포넌트를 한 번에 표시

<script src="https://gist.github.com/degan85/17a79715ea92bdc8291538720758531d.js?file=named-view.html"></script>

---

## Reference

[Do it! Vue.js 입문](http://www.yes24.com/24/goods/58206961)
