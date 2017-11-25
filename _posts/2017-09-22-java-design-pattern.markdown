---
layout: post
title:  "Design Pattern"
author: Degan
categories: Design_Pattern
tags:	java design pattern 
comments: true
---

**Design Pattern**에 무조건 얽매이면 안 좋지만 java를 공부하는 입장에서 한 번쯤은 진지하게 봐야 한다고 생각합니다. 시중에 많은 책이 있어서 어떤 책으로 공부를 해나갈지 고민이 되었습니다. 

GoF 책이 워낙 유명하지만 소스코드가 java가 아니고 무엇보다도 제가 읽기에는 어렵다고 판단이 들었습니다. 서점에서 책을 보던 중 결국 결정을 내리지 못하고 **Head First Design Patterns**[^1]과 **Java 언어로 배우는 디자인 패턴 입문**[^2] 두 권을 모두 샀습니다. 한 권을 결정하기에는 서로 가진 장점이 달라서 욕심을 냈는데 병행하면서 공부하려고 합니다.

---

## 왜 써야 하는가?

* 용어를 이해하고 나면 다른 개발자와 더 쉽게 대화가 가능.
* 패턴 수준에서 생각할 수 있게 해줘서 설계의 수준이 높아짐.
* 디자인은 예술! 많은 사람의 오랜 시간 동안의 고민이 녹아있음.

---

## 디자인 원칙

1. 바뀌는 부분은 따로 뽑아서 캡슐화.
2. 구현이 아닌 인터페이스에 맞춰서 프로그래밍.
	* 상위 형식에 맞춰서 프로그래밍.
	* 다형성을 활용.
	* 상위 형식의 인스턴스를 만드는 과정을 (`new ~`)처럼 직접 코드로 만들지 않고, 구체적으로 구현된 객체를 실행 시 대입하는 것이 좋음.(ex. `a.getInstance()`)
3. 상속보다는 구성을 활용.
	* `A에는 B가 있다.`
	
---

## Reference

[^1]:[Head First Design Patterns](http://www.hanbit.co.kr/store/books/look.php?p_code=B9860513241)

[^2]:[Java 언어로 배우는 디자인 패턴 입문](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788931436914)
