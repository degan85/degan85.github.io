---
layout: post
title:  "javascript, jQuery selector"
author: Degan
categories: javascript
tags:	javascript HTMLCollection nodelist jquery
comments: true
---
저는 DOM에 접근할때 jQuery를 많이 사용합니다. javascript보다 손쉽게 접근이 가능해서 그런것 같습니다. 최근 javascript를 공부하면서 javascript의 라이브러리인 jQuery의 선택자와, 순수 javascript 선택자는 완전히 동일한 결과를 가져오는지 의문이 들었습니다.

[jQuery와 DOM 첫걸음][jQuery와 DOM 첫걸음]에서 많은 정보를 얻었습니다. 현재 글은 대부분 위의 글을 읽고 정리한 내용입니다.

### 1. **jQuery**와 **JavaScript**로 DOM에 접근할때 차이가 없을까?

다음은 `document.getElementsByTagName()`(javascript), `document.querySelectorAll()`(javascript), `$()`(jQuery)가 반환하는 객체를 Chrome DevTools을 통해 본 결과입니다.

<script src="https://gist.github.com/degan85/f9f8e4f45744eb7645e72f73338897a1.js"></script>

순수 javascript 선택자를 이용한 결과는 `native javascript객체`를 반환하고 jQuery 선택자는 `jQuery객체`를 반환하는 것을 알 수 있습니다.

|Selector|  반환객체|
|---------|----------|
|getElementsBy~|HTMLCollection|
|querySelectorAll()|NodeList|
|$()|Object(0)|

### 2. HTMLCollection and nodelist and jquery

> The real reason why **getElementsByTagName()** is faster than **querySelectorAll()** is because of the difference between **live** and **static** NodeList objects. 

>...

> Live NodeList objects can be created and returned faster by the browser because they don’t have to have all of the information up front while **static NodeLists need to have all of their data from the start.**
[^1]

getElementsByTagName()가 querySelectorAll()보다 빠른 이유는 **live nodeList object**와
**static nodeList object**의 차이때문이라고 합니다. 

그리고 static NodeLists가 더 느린 이유는 **시작할때 모든 정보를 찾아야해서** 라고 하는데요. 위 글에서는 이것을  **snapshot of the document’s state**이라고 하는 것 같습니다.

제가 영어를 잘 못하니 직접 읽어 보시길 바랍니다.

**live**와 **static**의 차이점을 알기 위해 글을 참고하여 테스트를 해봤습니다.

<script src="https://gist.github.com/degan85/9fa8cba33379c79f5e9d928402e512b4.js"></script>

실제로 결과를 보면 `getElementsByTagName`로 `div`에 접근한 `divs_getElement`의 갯수는 **실시간**으로 늘어나서 출력되고 있습니다.

하지만 **querySelectorAll**로 접근한 `div_querySelect`와 **JQuery**로 접근한 `div_jquery`의 출력된 갯수는 `div`가 증가되어도 출력되지 않는 것을 볼 수 있습니다. 

### 3. jQuery Extensions

속도적인 측면에서 javascript 내부 메소드를 비교해보면 getElementsByTagName()가 빠릅니다.

그렇다면 jQuery 선택자의 경우 속도는 어떨까요?

jQuery가 선택자를 사용하는 방법에 따라 내부적으로 처리가 다르게 일어납니다. 속도적인 문제때문에 가능하다면 javascript 내장 메소드를 사용하려고 합니다. 하지만 내장 메소드를 통한 접근이 불가능하다면 jQuery가 직접 DOM tree를 순회하는 선택자를 사용합니다.

예를 들면 `:last`, `:first`, `:even`, `:eq()`등이 있습니다. 이 선택자들은 CSS 표준 선택자에 포함되지 않은 jQuery만의 확장된 선택자입니다. jQuery API를 보면 **jQuery Extension Selector**[^2]에 대해 기술되어 있습니다. 

그리고 가장 좋은 성능을 내려면 jQuery만의 확장된 선택자를 사용하기 보다 먼저 순수 CSS 선택자만을 사용하고, filter()를 사용하라고 권장합니다.

### 4. Summary

* 순수 javascript 선택자와 jQuery 선택자는 다른 객체를 반환합니다.
* 순수 javascript 선택자는 live한 객체와 static한 객체를 반환할 수 있습니다.
* live한 객체는 HTMLCollection로, static한 객체는 nodelist로 표현됩니다.
* getElements...은 접근만 하고 모든 해당 정보를 찾지 않아서 제일 빠릅니다.
* jquery는 접근이 가능하면 순수 javascript 내장 메소드를 사용하려고 합니다.(static)
* 위가 불가능하다면 jQuery만의 확장된 선택자를 사용하고 이는 성능에 영향을 줍니다.

### 5. Conclusion

* 성능을 우선시 한다면 되도록 순수 javascript 선택자를 이용합니다.
* elements에 접근만을 할 경우 getElements...을 사용합니다.
* snapshot of results나 more complex CSS query가 필요한 경우 querySelectorAll()를 사용합니다.
 
---
## Reference
[^1]:[Why is getElementsByTagName() faster than querySelectorAll()](https://www.nczonline.net/blog/2010/09/28/why-is-getelementsbytagname-faster-that-queryselectorall/)
[^2]:[Category: jQuery Extensions](http://api.jquery.com/category/selectors/jquery-selector-extensions/)
[jQuery와 DOM 첫걸음]:http://www.nextree.co.kr/p9747/
[HTMLCollection and nodelist and jquery]:https://degan85.github.io/javascript/2017/08/31/HTMLCollection-nodelist-Jquery.html
