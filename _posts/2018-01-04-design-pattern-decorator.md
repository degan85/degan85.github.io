
---
layout: post
title:  "[Head First Design Patterns] Decorator Pattern"
author: Degan
categories: Design_Pattern
tags:	java design pattern decorator
comments: true
---

## 문제 상황
- 상속을 통해서만 클래스를 관리할 경우

![observer pattern](http://snowdeer.github.io/assets/design-pattern-headfirst/decorator-badcase.png)
	- [출처: 디자인 패턴 UML 모음(Headfirst 디자인 패턴 책 발췌)](http://snowdeer.github.io/designpattern/2016/09/21/design-pattern-head-first-book-uml-list/) 

## OCP(Open-closed-Principle)
- 클래스 확장에 대해서는 열려 있어야 함
- 코드 변경에 대해서는 닫혀 있어야 함
- 기존 코드는 건드리지 않은 채로 확장을 통해 새로운 행동을 추가

## Decorator Pattern

![observer pattern](http://snowdeer.github.io/assets/design-pattern-headfirst/decorator-pattern.png)

- 객체에 추가적인 요건을 동적으로 첨가
- 서브클래스를 만드는 것을 통해 기능을 유연하게 확장
- 데코레이터 객체를 "래퍼" 객체로 생각
- 데코레이터는 자신이 장식하고(감싸고) 있는 객체에게 
- 어떤 행동을 **위임하는 것** 외에 원하는 **추가적인 작업**을 수행할 수 있음

- java.io 패키지의 많은 부분이 Decorator pattern을 바탕으로 만들어짐


## 스타버지 sample 

![observer pattern](http://snowdeer.github.io/assets/design-pattern-headfirst/starbuzz-decorator.png)

- 상속 대신 구성성을 사용할 계획이 아니었나?
	- 상속을 이용해서 형식을 맞추는 것일 뿐
	- 상속을 통해서 행동을 물려받는 게 목적이 아님
	- 행동은 기본 구성 요소로 하고
	- 다른 데코레이터 등을 인스턴스 변수에 저장하는 식으로 연결

---

- Beverage (음료)
	- 추상 구성요소

```java
public abstract class Beverage {
	String description = "제목 없음";
	
	public String getDescription() {
		return description;
	}
	
	public abstract double cost();
}
```
---

- Espresso
	- 구상 구성요소

```java
public class Espresso extends Beverage{
	
	public Espresso() {
		
		// Beverage로부터 상속 받은 인스턴스 변수
		description = "에스프레소";
	}

	public double cost() {
		return 1.99;
	}
}
```

- HouseBlend, DarkRoast 등 같은 방법으로 

---

- ConimentDecorator(첨가물)
	- 추상 데코레이터

```java

public abstract class CondimentDecorator extends Beverage{
	
	// 모든 첨가물 테코레이터에서 getDescription() 메소드를 새로 구현하게
	public abstract String getDescription();
}
```

- Beverage 객체가 들어갈 자리에 들어갈 수 있어야 하므로
- Beverage 클래스를 확장

---

- Mocha
	- 구상 데코레이터

```java
public class Mocha extends CondimentDecorator{
	
	Beverage beverage;
	
	public Mocha(Beverage beverage) {
		this.beverage = beverage;
	}
	
	public String getDescription() {
		return beverage.getDescription() + ", 모카";
	}
	
	public double cost() {
		return .20 + beverage.cost();
	}
}
```

- Mocha는 데코레이터이기 때문에 CondimentDecorator를 확장
- 데코레이터의 생성자에 감싸고자 하는 음료 객체를 전달
- 인스턴스 변수에 감싸고자 하는 음료를 저장

- 두유, 휘핑 크림, 스팀 밀크도 같은 방식으로 

---

- 커피 주문

```java
public class StarbuzzCoffee {
	public static void main(String[] args) {
		
		Beverage beverage = new Espresso();
		System.out.println(beverage.getDescription() + " $"+beverage.cost());
		
		Beverage beverage2 = new DarkRoast();
		beverage2 = new Mocha(beverage2);
		beverage2 = new Mocha(beverage2);
		beverage2 = new Whip(beverage2);
		System.out.println(beverage2.getDescription() + " $"+beverage2.cost());
		
		Beverage beverage3 = new HouseBlend();
		beverage3 = new Soy(beverage3);
		beverage3 = new Mocha(beverage3);
		beverage3 = new Whip(beverage3);
		System.out.println(beverage3.getDescription() + " $"+beverage3.cost());
	}
}
```

- 결과

```console
에스프레소 $1.99
다크 로스트, 모카, 모카, 휘핑 크림 $1.49
하우스 블렌드 커피, 두유, 모카, 휘핑 크림 $1.34
```

---

## Reference

[Head First Design Patterns](http://www.hanbit.co.kr/store/books/look.php?p_code=B9860513241)
