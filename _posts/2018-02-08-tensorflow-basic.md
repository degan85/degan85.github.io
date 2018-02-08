---
layout: post
title: " Tensorflow tutorial 기초"
author: Degan
categories: tensorflow
tags:	tensorflow deeplearing
comments: true
---

## Import

```python
import tensorflow as tf
import numpy as np
```

## Data setting

- 데이터는 털과 날개 유무
- 레이블
	- 0 : 기타
	- 1 : 포유류
	- 2 : 조류
	- 원-핫 인코딩 형태 구성

```python
# [털, 날개]
x_data = np.array([
    [0, 0], [1, 0], [1, 1], [0, 0], [0, 0], [0, 1]
])
y_data = np.array([
    [1, 0, 0],  # 기타
    [0, 1, 0],  # 포유류
    [0, 0, 1],  # 조류
    [1, 0, 0],
    [1, 0, 0],
    [0, 0, 1]
])
```

## Model 만들기

- X(데이터), Y(레이블)을 placeholder로 만듦
	- name을 사용하면 이름 부여 가능
	- 텐서보드 이용시 편리
- 가중치(W)와 편향(b)는 -1 ~ 1 사이의 균등분포를 가진 무작위 값으로 초기화
- 입력층, 은닉층, 출력층의 개수를 행렬 계산이 가능하게  맞춰야 함
- 활성화 함수는 Relu 사용

```python
X = tf.placeholder(tf.float32)
Y = tf.placeholder(tf.float32)

# 입력층 [특징, 은닉층]
# 출력측 [은닉층, 분류 수]
W1 = tf.Variable(tf.random_uniform([2, 10], -1., 1.))
W2 = tf.Variable(tf.random_uniform([10, 3], -1., 1.))

b1 = tf.Variable(tf.zeros([10]))
b2 = tf.Variable(tf.zeros([3]))


L1 = tf.add(tf.matmul(X, W1), b1)
L = tf.nn.relu(L1)

model = tf.add(tf.matmul(L1, W2), b2)
```

## 손실함수 및 최적화

- 손실값은 크로스 엔트로피 사용
- Adam 알고리즘을 사용
	- 손실값을 최소화 하기 위해
	- W와 b를 최적화
	- 하이퍼하라미터인 **학습률(learning_rate)**을 잘 튜닝해야 함

```python
# 크로스 엔트로피
# cost = tf.reduce_mean(-tf.reduce_sum(Y * tf.log(model), axis=1))
cost = tf.reduce_mean(tf.nn.softmax_cross_entropy_with_logits_v2(labels=Y, logits=model))

optimizer = tf.train.AdamOptimizer(learning_rate=0.01)
train_op = optimizer.minimize(cost)
```

## Tensorflow 세션 초기화

- 그래프 생성
	- 여기서 그래프는 텐서들의 연산 모음
	- 텐서와 텐서의 연산들을 먼저 정의해서 그래프 만듦
- 그래프 실행
	- **원하는 시점**에 실제 연산 수행
	- **지연 실행**
	- 그래프 실행은 `Session` 안에서 이뤄짐
- 실행 전 변수들을 초기화 해줌

```python
init = tf.global_variables_initializer()
sess = tf.Session()
sess.run(init)
```

## 학습

- 특징과 레이블 데이터를 이용해 학습을 100번 진행
- X와 Y에 x_data와 y_data 넣음
- 학습 도중 10번에 한 번씩 손실값 출력

```python
for step in range(100):
    sess.run(train_op, feed_dict={X: x_data, Y: y_data})

    if(step + 1) % 10 == 0:
        print(step+1, sess.run(cost, feed_dict={X: x_data, Y: y_data}))
```

## 학습 결과

- 예측값인 model을 출력하면 확률로 나옴
	- ex) [0.2, 0.7, 0.1]
	- 합이 1
	- 가장 큰 index를 찾아주는 argmax 함수 사용
- is_correct 
	- 예측값과 레이블 비교
	- true & false
- accuracy
	- tf.cast 함수를 사용 다시 0, 1로 변환
	- reduce_mean 함수로 평균 구함

```python
prediction = tf.argmax(model, axis=1)
target = tf.argmax(Y, axis=1)
print('예측값:', sess.run(prediction, feed_dict={X: x_data}))
print('실제값:', sess.run(target, feed_dict={Y: y_data}))

is_correct = tf.equal(prediction, target)
accuracy = tf.reduce_mean(tf.cast(is_correct, tf.float32))
print('정확도: %.2f' % sess.run(accuracy * 100, feed_dict={X: x_data, Y: y_data}))
```

## 결과

```
10 0.50371915
20 0.2590824
30 0.13661371
40 0.07838756
50 0.04937454
60 0.03441563
70 0.025879657
80 0.02045995
90 0.016735232
100 0.014022327
예측값: [0 1 2 0 0 2]
실제값: [0 1 2 0 0 2]
정확도: 100.00
```

---

## Reference

[골빈해커의 3분 딥러닝](http://www.hanbit.co.kr/store/books/look.php?p_code=B7257101308)
