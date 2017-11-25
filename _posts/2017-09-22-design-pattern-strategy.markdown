---
layout: post
title:  "Strategy Pattern"
author: Degan
categories: Design_Pattern
tags:	java design pattern 
comments: true
---

소스코드와 글의 내용은 **Head First Design Patterns**[^1]와 **Java 언어로 배우는 디자인 패턴 입문**[^2]를 참고하여 작성했습니다.

---

## Strategy Pattern

* 알고리즘 군을 정의하고 각각을 캡슐화하여 교환해서 사용할 수 있도록 만듦.
* 알고리즘을 사용하는 클라이언트와 독립적으로 알고리즘을 변경할 수 있음.

---

## UML

![strategy pattern uml](http://www.oodesign.com/images/design_patterns/behavioral/strategy_implementation_-_uml_class_diagram.gif)
출처:http://www.oodesign.com/strategy-pattern.html

#### Strategy
* 전략을 이용하기 위한 인터페이스(API) 결정.

#### ConcreteStrategy
* 구체적인 전략의 역할.
* strategy의 인터페이스(API)를 실제로 구현.
* 작전, 방책, 방법, 알고리즘을 실제로 프로그래밍.

#### Context
* Strategy를 이용하는 역할
* Strategy의 인터페이스(API)를 호출해 이용

---

## Code

#### Context 슈퍼 클래스 Duck

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=Duck.java"></script>

오리들의 슈퍼 클래스 Duck입니다. 모든 오리의 공통 기능은 Duck에 넣어서 상속시키면 문제가 없지만 오리마다 다른 행동을 보여야 하는 부분을 추가할 때는 상속으로 처리하면 문제가 발생합니다.

예를 들면 fly()기능을 추가하면 장난감 오리들도 하늘을 날 수 있게 오류가 발생하게 됩니다. 

변경되는 부분들만 캡슐화해서 알고리즘 군으로 정의하고 사용하면 좋을 것 같습니다.

{% highlight java %}
	FlyBehavior flyBehavior;
	QuackBehavior quackBehavior;
{% endhighlight %}

위의 코드처럼 변경돼야 하는 행동들을 인터페이스로 정의합니다.

{% highlight java %}
	public void fly() {
		flyBehavior.fly();
	}
{% endhighlight %}

그리고 위와 같이 fly()를 정의합니다.

#### Context 슈퍼 클래스 상속받은 오리들

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=MallardDuck.java"></script>	

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=RedheadDuck.java"></script>

슈퍼 클래스 Duck을 상속받은 MallardDuck과 RedheadDuck 클래스입니다. 생성될 때 나는 행동과 우는 행동을 각자 맞게 정의합니다. (**전략을 결정**) 코드를 보면 알 수 있듯이 청둥오리는 날개로 날고, 꽥꽥 웁니다.

진행 중 행동의 수정을 원한다면 슈퍼 클래스 Duck에 있는 `setFlyBehavior()`나 `setQuackBehavior()`를 호출합니다.
	
#### Strategy 행동 interface
		
모두 공통으로 변경되거나 추가되면 안 되는 행동들을 인터페이스로 만듭니다.

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=FlyBehavior.java"></script>

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=QuackBehavior.java"></script>
		
#### ConcreteStrategy 나는 행동들

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=FlyWithWing.java"></script>

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=FlyNoWay.java"></script>

다음과 같이 나는 행동들을 만듭니다. 요청이 와서 나는 행동이 추가될 경우 인터페이스를 구현하는 클래스를 또 추가해서 만들면 됩니다.

<script src="https://gist.github.com/degan85/871ad5107a8cf6cc118576852824e04d.js?file=FlyRocketPowered.java"></script>
	
---

## 실행

{% highlight java %}
public static void main(String[] args) {

	Duck mallardDuck = new MallardDuck();
	mallardDuck.fly();
	mallardDuck.quack();
	mallardDuck.setFlyBehavior(new FlyNoWay());
	mallardDuck.fly();

	Duck redHeadDuck = new RedheadDuck();
	redHeadDuck.fly();
	redHeadDuck.quack();
}
{% endhighlight %}

{% highlight java %}

날개로 날아		// mallardDuck.fly();
꽥꽥			// mallardDuck.quack();
못 날아			// mallardDuck.setFlyBehavior(new FlyNoWay()); 이후 mallardDuck.fly();

로케트 파워로 날라!!	// redHeadDuck.fly();
삑삑				// redHeadDuck.quack();
{% endhighlight %}
	
---

## Reference

[^1]:[Head First Design Patterns](http://www.hanbit.co.kr/store/books/look.php?p_code=B9860513241)

[^2]:[Java 언어로 배우는 디자인 패턴 입문](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788931436914)
