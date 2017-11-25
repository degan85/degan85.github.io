---
layout: post
title:  "JMH을 이용한 Collection 비교"
author: Degan
categories: JAVA
tags:	collection list set queue jmh
comments: true
---

 **collection** 객체를 대략 정리하고 책에[^1] 있는 예제를 직접 실행해 보았습니다. 실행 결과는 주석으로 써넣었습니다.
 
---

## collection 및 map 

* Collection : 가장 상위 인터페이스.
* Set : 중복을 허용하지 않는 집합을 처리하기 위한 인터페이스.
* SortedSet : 오름차순을 갖는 Set 인터페이스.
* List : 순서가 있는 집합을 처리하기 위한 인터페이스. 인덱스로 지정해서 값을 찾을 수 있음. 중복을 허용.
* Queue: 여러 개의 객체를 처리하기 전에 담아서 처리할 때 사용하기 위한 인터페이스. FIFO(First In First Out)
* Map : 키와 값의 쌍으로 구성된 객체의 집합을 처리하기 위한 인터페이스. 키의 중복을 허용하지 않음.
* SortedMap : 키를 오름차순으로 정렬하는 Map 인터페이스.

---

## 특징

#### Set

- TreeSet은 데이터를 담으면서 동시에 정렬하기 때문에 성능상 HashSet보다 느리다.
- 하지만 단일 값 검색과 범위검색이 매우 빠름.

#### List

- **ArrayList**와 **Vector**는 실제로 그 안에 **배열**을 사용.
- 데이터를 읽어오고 저장하는 데는 효율이 좋음.
- 용량을 변경해야 할 때는 새로운 배열을 생성해서 복사해야 함.(효율이 떨어짐)
- 그 배열의 0번째 값을 삭제하면 첫 번째에 있던 값이 0번째로 와야만 함.
- 따라서 앞의 값을 삭제하면 모두 한 칸씩 이동하므로 속도가 느려짐.(`System.arraycopy()` 호출)
- 위의 단점을 보완하기 위해 **linkedList**가 고안됨.(gist 코드에서 확인)
- 하지만 읽기 접근 시간은 LinkedList가 느리다.

- ArrayList를 생성 할 때 초기 값을 써주면 1.5정도 빠름.
- ArrayList는 여러 스레드에서 접근할 경우 문제가 발생할 수 있음.
- Vector는 여러 스레드에서 접근을 방지하기 위해 get() 메서드에 syncronized가 선언되어있음. 
- 그래서 arrayList보다 속도면에서 성능 저하가 발생 함.

#### Queue

- JDK 5.0 에서 추가됨
- List가 있는데 굳이 Queue가 필요한 이유는 List는 편집 시 속도처리 단점 때문.

#### Map

- **hashing**을 사용하기 때문에 많은 양의 데이터를 검색하는데 뛰어남.
- hashing은 **hash function**을 이용해서 데이터를 **hash table**에 저장하고 검색하는 기법
- 저장할 데이터의 키를 해시함수에 넣으면 배열의 한 요소를 얻게 됨.(해시코드)
- 다시 그곳에 연결되어 있는 LinkedList에 저장 됨.
- linkedList의 검색 속도가 느리기 때문에 중복된 해시코드의 반환을 최소로 해야함.

#### 동기화

- JDK 1.2 이전에는 자체적으로 동기화 처리가 되있었으나 성능 저하로 필요한 경우 사용하게 변경.
- Collections 클래스에서 최신 버전 클래스들의 동기화를 지원하기 위해 synchronized로 시작하는 메서드들이 있음.
- Map의 경우 키 값들을 Set으로 가져와 Iterator를 통해 데이터를 처리할 경우가 있는데 이때 ConcurrentModificationException 예외가 발생할 수 있음.
- java.util.concurrent 패키지를 확인해 해결.[^2]

---

## Gist

<script src="https://gist.github.com/degan85/80dbee15ae9a08746976db123710ecf2.js"></script>

## Reference

[^1]:[자바 성능 튜닝 이야기](http://www.kyobobook.co.kr/product/detailViewKor.laf?barcode=9788966260928)
[^2]:[java.util.concurrent.locks](https://vnthf.github.io/blog/Java-java.util.concurrent.locks/)
[JAVA의 정석]()
