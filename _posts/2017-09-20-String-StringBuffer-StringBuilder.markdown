---
layout: post
title:  "String, StringBuffer, StringBuilder 비교"
author: Degan
categories: JAVA
tags:	jmh String StringBuffer StringBuilder
comments: true
---

**String**의 경우 `+`연산을 실행하면 연산 결과를 새로운 주소에 새로운 객체로 만들어집니다.
이런 작업이 반복적으로 수행되면 메모리를 많이 사용하게 되고 응답 속도도 영향을 미치게 됩니다. 실제 jmh를 사용해서 속도를 비교해보겠습니다.

---
## 속도 비교

<script src="https://gist.github.com/degan85/7b636be7a9ea46119502a606321e4f01.js"></script>

밑에 주석 부분을 다시 보면 아래와 같습니다.

{% highlight java %}
# Run complete. Total time: 00:00:31

Benchmark                                Mode  Cnt      Score     Error  Units
StringAndStringBuffer.StringBufferTest   avgt    5     77.061 ±  17.772  us/op
StringAndStringBuffer.StringBuliderTest  avgt    5     78.279 ±  10.488  us/op
StringAndStringBuffer.StringTest         avgt    5  12087.545 ± 350.424  us/op
{% endhighlight %}

예상 했던 대로 **String**의 속도가 많은 차이를 보이는 것을 알 수 있습니다.

---
## 컴파일

실제 String을 `+`연산 할 경우 JDK 5.0 이상에서는 컴파일러가 알아서 StringBuilder로 변환해서 수행합니다. 하지만 반복 루프를 사용해서 문자열을 계속 더할 때는 계속 추가한다는 사실에는 변화가 없습니다.[^1] 그래서 되도록이면 StringBuffer나 StringBuilder를 사용하는것이 좋습니다.

---
## CharSequnce 인터페이스

**CharSequnce**는 인터페이스라서 객체를 생성할 수 없습니다. 이 인터페이스로 구현한 클래스로는 CharBuffer, String, StringBuffer, StringBuilder가 있으며, StringBuffer나 StringBuilder로 생성한 객체를 전달할 때 사용합니다.

{% highlight java %}

public class StringBufferTest {
    public static void main(String[] args) {
        StringBuilder sb = new StringBuilder();
        sb.append("ABCDE");
        StringBufferTest sbt = new StringBufferTest();
        
        sbt.check(sb);
    }
    
    public void check(CharSequence cs) {
        StringBuffer sb = new StringBuffer(cs);
        System.out.println("sb.length : "+sb.length());
    }
}

{% endhighlight %}

StringBuffer나 StringBuilder로 값을 만든 후 굳이 toString을 수행하여 필요 없는 객체를 만들어 넘겨주는 것 보다 메모리에 효율적입니다.

---
## 정리

#### String
 * 짧은 문자열을 더할 경우 사용.

#### StringBuffer
 * 스레드에 안전하게 설계되어 있음.
 * 스레드에 안전한 프로그램이 필요할 때 사용. 
 * 개발 중인 시스템의 부분이 스레드에 안전한지 모를 경우 사용. 
 * 만약 클래스에 static으로 선언한 문자열을 변경할 때 사용
 * singleton으로 선언된 클래스에 선언된 문자열일 경우에 사용.

#### StringBuilder
 * 단일 스레드에서의 안전성만을 보장.
 * 스레드에 안전한지의 여부와 전혀 관계 없는 프로그램을 개발할 때 사용하면 좋음.
 * 지역변수로 사용하지 좋음.

 
---
## Reference

[^1]:[자바 성능 튜닝 이야기](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788966260928)

