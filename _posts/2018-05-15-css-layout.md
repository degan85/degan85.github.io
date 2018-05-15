---
layout: post
title: "CSS layout"
author: Degan
categories: CSS
tags:	css flow position display float
comments: true
---

## 엘리먼트가 배치되는 방식

- display
  - block
    - 위에서 아래로 쌓임
    - <p>, <div> ...
  - inline
    - 우측으로 쌓임
    - <span>, <a>, <strong> ...

- position
  - static
    - 기본(그냥 순서대로 배치)
  - absolute
    - 기준점에 따라 위치
    - 기준점은 상위엘리먼트로 단계적으로 찾아가는데 static이 아닌 position이 기준점
      - 보통 위에 relative를 부모를 둠
      - 부모가 모두 static이면 body를 기준으로 함
    - top, left값을 0이라도 줘야 함
  - relative
    - 원래 자신이 위치해야 할 곳을 기준으로 이동
  - fixed
    - viewport(전체화면) 좌측 맨위를 기준으로 동작
    - 광고같이 계속 같은 위치에 있음

- float
  - 원래 flow에서 벗어나 둥둥 떠 있음
  - 뒤에 block엘리먼트가 float 된 엘리먼트를 의식하지 못하고 중첩돼서 배치 됨
  - left
  - right

## layout 구현방법은?

- 전체 레이아웃은 float를 잘 사용해서 2단, 3단 컬럼 배치를 구현
- 최근에는 css-grid나 flex 속성 등 layout을 위한 속성을 사용하기 시작했으며 브라우저 지원범위를 확인해서 사용
- 특별한 위치에 배치하기 위해서는 position absolute를 사용하고, 기준점을 relative로 설정
- 네비게이션과 같은 엘리먼트는 block 엘리먼트를 inline-block으로 변경해서 가로로 배치하기도 함 
- 엘리먼트안의 텍스트의 간격과 다른 엘리먼트간의 간격은 padding과 margin 속성을 잘 활용해서 위치

## Reference

[Full-Stack Web Developer](http://www.edwith.org/boostcourse-web/lecture/16677)
