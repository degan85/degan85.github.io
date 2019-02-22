---
layout: post
title:  "Clojure 시퀀스1"
author: Degan
categories: clojure seq
tags:   clojure lein seq	
comments: true
---

## 시퀀스 세가지 기본 특성

<script src="https://gist.github.com/degan85/c5b8d28ca9828ba71466d057cdc18679.js"></script>

<script src="https://gist.github.com/degan85/c5b8d28ca9828ba71466d057cdc18679.js?sequence.clj
"></script>

 - conj와 다른 관련된 함수들이 자료구조의 내부 구현과 관련해 가장 효율적으로 선택
 - list는 맨 앞으로 삽입

```clojure
(conj '(1 2 3) :a)
;(:a 1 2 3)
```

 - 벡터는 맨 뒤로 삽입
 
```clojure
(conj '[1 2 3] :a)
(1 2 3 :a)
```

 - 클로저의 시퀀스 라이브러리는 규모가 큰(심지어 무한대) 시퀀스를 다루는데 매우 적합
 - 대부분의 클로저 시퀀스가 실제로 꼭 필요해질 때까지 연산을 **미루기** 때문
 - 또한 클로저의 시퀀스는 **변경 불가능**

## 시퀀스 생성
### range

`(range start? end step?)`

```clojure
(range 10)
;(0 1 2 3 4 5 6 7 8 9)

(range 1 25 2)
;(1 3 5 7 9 11 13 15 17 19 21 23)
```

### repeat 

`(repeat n x)`

```clojure
(repeat 5 1)
;(1 1 1 1 1)
```

### iterate

`(iterate f x)`
`(take n sequence)`

```clojure
(take 10 (iterate inc 1))
;(1 2 3 4 5 6 7 8 9 10)
```
 
### cycle 여러 컬렉션을 받아서 무한히 반복
`(cycle coll)`

```clojure
(take 10 (cycle (range 3)))
;(0 1 2 0 1 2 0 1 2 0)
```

### interleave 여러 컬렉션을 받아 값이 차례로 교차되는 새로운 컬랙션 생성

`(interleave & colls)`

```clojure
(defn whole-numbers [] (iterate inc 1))
(interleave (whole-numbers) ["A" "B" "C" "D"])
;(1 "A" 2 "B" 3 "C" 4 "D")
```

### interpose 컬렉션과 구분자를 인자로 받아, 각 원소 사이에 삽입해 새 시퀀스 생성

`(interpose separator coll)`

```clojure
(interpose "," ["apple" "bananas" "grapes"])
;("apple" "," "bananas" "," "grapes")

(apply str (interpose "," ["apple" "bananas" "grapes"])) 
;"apples,bananas,grapes"
```

## etc

`(hash-set & elements)`

```clojure
(set [1 2 3])
;#{1 3 2}

(hash-set 1 2 3)
;#{1 3 2}
```

`(vector & elements)`

```clojure
(vec (range 3))
;[0 1 2]
```

---

## Reference

[Programming Clojure](http://www.yes24.com/Product/goods/3907543)
