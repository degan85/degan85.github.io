---
layout: post
title:  "Clojure 시퀀스2"
author: Degan
categories: clojure
tags:   clojure lein seq	
comments: true
---

## 시퀀스 필터링

### filter
`(filter pred coll)`

```clojure
(take 10 (filter even? (whole-numbers)))
;(2 4 6 8 10 12 14 16 18 20)

(take 10 (filter odd? (whole-numbers)))
;(1 3 5 7 9 11 13 15 17 19)
```

### take-while 서술식을 컬렉션의 각 원소에 적용해서 거짓이 나타나기 전까지
`(take-while pred coll)`

```clojure
(take-while (complement #{\a\e\i\o\u}) "the-quick-brown-fox")
;(\t \h)
```

### split-at, split-with
`(split-at index coll)`
`(split-with pred coll)`

```clojure
(split-at 5 (range 10))
;[(0 1 2 3 4) (5 6 7 8 9)]

(split-with #(<= % 10) (range 0 20 2))
;[(0 2 4 6 8 10) (12 14 16 18)]
```

## 시퀀스 서술식
- 필터링 함수는 서술식을 받아 시퀀스를 반환

### every
`(every? pred coll)`

```clojure
(every? odd? [1 3 5])
;true

(every? odd? [1 3 5 8])
;false
```

### some
`(some pred coll)`

```clojure
(some even? [1 2 3])
;true

(some even? [1 3 5])
;nil
```

- some은 물음표로 끝나지 않음(서술식이 아님)
- 처음으로 서술식이 만족되면 true 대신 서술식이 **적용된 결과**를 반환

```clojure
(some identity [nil false 1 nil 2])
;1
```

### not-every, not-any
`(not-every? pred coll)`
`(not-any? pred coll)`

```clojure
(not-every? even? (whole-numbers))
;true

(not-any? even? (whole-numbers))
;false
```
---

## Reference

[Programming Clojure](http://www.yes24.com/Product/goods/3907543)
