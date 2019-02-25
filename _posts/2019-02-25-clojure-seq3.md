---
layout: post
title:  "Clojure 시퀀스3"
author: Degan
categories: clojure
tags:   clojure lein seq	
comments: true
---

## 시퀀스 변환
### map
`(map f coll)`
- 컬렉션 coll과 함수 f를 받음
- coll의 각 원소에 f를 적용한 결과로 만들어진 시쿼스를 반환

```clojure
(map #(format "<p>%s</p>" %) ["the" "quick" "brown" "fox"])
;"<p>the</p> <p>quick</p> <p>brown</p> <p>fox</p>"

(map #(format "<%s>%s</%s>" %1 %2 %1) 
     ["h1" "h2" "h3" "h1"] ["the" "quick" "brown" "fox"])
;"<h1>the</h1> <h2>quick</h2> <h3>brown</h3> <h1>fox</h1>"
```

### reduce
`(reduce f coll)`
- f는 인자를 두 개 받는 함수여야 함
- coll의 첫 두 원소에 f를 적용한 후
- 그 결과와 coll의 세 번째 원소에 다시 f를 적용

```clojure
(reduce + (range 1 11))
;55

(reduce * (range 1 11))
;3628800
```

### for
`(for [binding-form coll-expr filter-expr? ...] expr)`

- 사실 모든 필터링 및 변환 함수의 원조격은 **리스트 해석(list comprehension)**
- 리스트 해석은 조건 제시법을 이용해서 기존 리스트에서 새로운 리스트를 만들어 냄
- 대개 리스트 해석은 다음 요소로 구성
  * 기존 리스트
  * 기존 리스트의 원소를 표현하는 데 필요한 변수
  * 기존 리스트의 원소가 만족시켜야 하는 서술식
  * 서술식을 만족시키는 원소에서 어떻게 결과 리스트의 원소를 만들어 낼지 기술하는 구문

- 클로저는 리스트 해석의 개념을 **시퀀스**해석으로 일반화 함

- for는 binding-form, coll-expr, filter-expr로 이루어진 벡터를 받음
- expr에 해당하는 시퀀스를 만들어 냄

```clojure
(for [word ["the" "quick" "brown" "fox"]]
  (format "<p>%s</p>" word))
;"<p>the</p> <p>quick</p> <p>brown</p> <p>fox</p>"

(take 10 (for [n (whole-numbers) :when (even? n)] n))
;(2 4 6 8 10 12 14 16 18 20)

;binding-form을 하나 이상 사용
(for [file "ABCDEFGH" rank (range 1 9)] (format "%c%d" file rank))
;("A1" "A2" ...(중략)... "H7" "H8")

(for [rank (range 1 9) file "ABCDEFGH"] (format "%c%d" file rank))
;("A1" "B1" ...(중략)... "G8" "H8")
```

- for의 진정한 강력함은 하나 이상의 binding-form을 사용할 때
- 바인딩 표현의 오른쪽에 있는 시퀀스의 원소부터 차례로 바인딩

## 지연 시퀀스와 무한한 시퀀스

- 대부분의 클로저 시퀀스는 연산을 **지연** 함
- 즉, 시퀀스의 원소가 실제로 필요해질 때까지 계산되지 않음
- **지연 시퀀스(lazy sequence)**는 다음과 같은 장점이 있음
  * 실제로 필요 없는 막대한 양의 연산이 있다면, 정말 필요할 때까지 미룰 수 있음
  * 메모리 용량을 초과하는 거대한 데이터 집합을 다룰 수 있음
  * I/O 역시 정말로 필요할 때까지 미뤄둘 수 있음

- 대부분의 클로저 시퀀스 함수는 지연 시퀀스를 반환

---

## Reference

[Programming Clojure](http://www.yes24.com/Product/goods/3907543)
