---
layout: post
title:  "try with resource"
date:   2017-06-15
author: Degan
categories: JAVA
tags:	java, resource
comments: true
---

I/O처리 작업중에 예외가 발생해도 resource를 닫아야 합니다. 그래서 `finally`안에 `close()`를 또 `try-catch`로 감싸줘야 합니다. 만일 `close()`를 실행하는데 예외가 생기면 그 예외를 처리해줘야 하기 때문입니다. 

하지만 **JDK1.7**이후부터는 다음과 같이 `try()`안에 인스턴스를 하면 `try()`를 벗어날때  알아서 `close()`를 호출해줍니다. 

<script src="https://gist.github.com/degan85/f7d535a87dad659b74c623207a3ee684.js"></script>

---

## Reference
[자바의 정석](http://www.kyobobook.co.kr/product/detailViewKor.laf?ejkGb=KOR&barcode=9788994492032)


