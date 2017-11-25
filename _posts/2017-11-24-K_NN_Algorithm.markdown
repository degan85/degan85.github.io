---
layout: post
title:  "K-Nearest Neighbors Algorithm"
author: Degan
categories: machine_learning
tags:	machine learning knn algorithm 
comments: true
---

```python
from IPython.display import display
import numpy as np
from matplotlib import font_manager, rc
import matplotlib.pyplot as plt
import pandas as pd
import mglearn
%matplotlib inline
font_name = font_manager.FontProperties(fname="C://Windows//Fonts/malgun.ttf").get_name()
rc('font', family=font_name)
```


```python

# 데이터셋을 만든다
X, y = mglearn.datasets.make_forge()
# 산점도를 그린다
mglearn.discrete_scatter(X[:, 0], X[:, 1], y)
plt.legend(["클래스 0", "클래스 1"], loc = 4)
plt.xlabel("첫 번째 특성")
plt.ylabel("두 번째 특성")
print("X.shape: {}".format(X.shape))
```

    X.shape: (26, 2)
    


![knn_png](https://degan85.github.io/assets/K-NN_1_1.png)




```python
X, y = mglearn.datasets.make_wave(n_samples=40)
plt.plot(X, y, 'o')
plt.ylim(-3, 3)
plt.xlabel("특성")
plt.ylabel("타깃")
```




    Text(0,0.5,'타깃')




![knn_png](https://degan85.github.io/assets/K-NN_2_1.png)



```python
from sklearn.datasets import load_breast_cancer
cancer = load_breast_cancer()
print("cancer.key(): \n{}".format(cancer.keys()))
```

    cancer.key(): 
    dict_keys(['data', 'target', 'target_names', 'DESCR', 'feature_names'])
    


```python
print("유방암 데이터의 형태: {}".format(cancer.data.shape))
```

    유방암 데이터의 형태: (569, 30)
    


```python
print("클래스별 샘플 개수:\n{}".format({n: v for n, v in zip(cancer.target_names, np.bincount(cancer.target))}))
```

    클래스별 샘플 개수:
    {'malignant': 212, 'benign': 357}
    


```python
print("특성 이름:\n{}".format(cancer.feature_names))
```

    특성 이름:
    ['mean radius' 'mean texture' 'mean perimeter' 'mean area'
     'mean smoothness' 'mean compactness' 'mean concavity'
     'mean concave points' 'mean symmetry' 'mean fractal dimension'
     'radius error' 'texture error' 'perimeter error' 'area error'
     'smoothness error' 'compactness error' 'concavity error'
     'concave points error' 'symmetry error' 'fractal dimension error'
     'worst radius' 'worst texture' 'worst perimeter' 'worst area'
     'worst smoothness' 'worst compactness' 'worst concavity'
     'worst concave points' 'worst symmetry' 'worst fractal dimension']
    


```python
from sklearn.datasets import load_boston
boston = load_boston()
print("데이터의 형태: {}".format(boston.data.shape))
```

    데이터의 형태: (506, 13)
    

## k-최근접 이웃

가장 가까운 훈련 데이터 포인트를 하나의 최근접 이웃으로 찾아 예측에 사용


```python
# 1 - 최근접 이웃
mglearn.plots.plot_knn_classification(n_neighbors=1)
```


![knn_png](https://degan85.github.io/assets/K-NN_9_0.png)



```python
# k(3) - 최근접 이웃
mglearn.plots.plot_knn_classification(n_neighbors=3)
```


![knn_png](https://degan85.github.io/assets/K-NN_10_0.png)



```python
from sklearn.model_selection import train_test_split
X, y = mglearn.datasets.make_forge()

X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)
```


```python
from sklearn.neighbors import KNeighborsClassifier
clf = KNeighborsClassifier(n_neighbors=3)
```


```python
clf.fit(X_train, y_train)
```




    KNeighborsClassifier(algorithm='auto', leaf_size=30, metric='minkowski',
               metric_params=None, n_jobs=1, n_neighbors=3, p=2,
               weights='uniform')




```python
print("테스트 세트 예측: {}".format(clf.predict(X_test)))
```

    테스트 세트 예측: [1 0 1 0 1 0 0]
    


```python
print("테스트 세트 정확도: {:.2f}".format(clf.score(X_test, y_test)))
```

    테스트 세트 정확도: 0.86
    

## KNeighborsClassifier 분석

이웃이 하나, 셋, 아홉 개일 때의 결정 경계


```python
fig, axes = plt.subplots(1, 3, figsize=(10, 3))

for n_neighbors, ax in zip([1, 3, 9], axes):
    # fit 메서드는 self 객체를 반환합니다.
    # 그래서 객체 생성과 fit 메서드를 한 줄에 쓸 수 있습니다.
    clf = KNeighborsClassifier(n_neighbors = n_neighbors).fit(X, y)
    mglearn.plots.plot_2d_separator(clf, X, fill=True, eps=0.5, ax=ax, alpha=.4)
    mglearn.discrete_scatter(X[:,0], X[:, 1], y, ax=ax)
    ax.set_title("{} 이웃".format(n_neighbors))
    ax.set_xlabel("특성 0")
    ax.set_ylabel("특성 1")
axes[0].legend(loc=3)
```




    <matplotlib.legend.Legend at 0xd012710>




![knn_png](https://degan85.github.io/assets/K-NN_17_1.png)


* 이웃을 하나 선택했을 때
    - 결정 경계가 훈련데이터에 가깝게 따라가고 있다. 
    - 복잡도 높다.
    - 과대적합(overfitting)
    
* 이웃의 수를 늘렸을 때
    - 경계는 더 부드러워진다.
    - 더 단순한 모델이 된다.
    - 복잡도가 낮아진다.
    - 과소적합(underfitting)


```python
from sklearn.datasets import load_breast_cancer

cancer = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(cancer.data, cancer.target, stratify=cancer.target, random_state=66)

training_accuracy = []
test_accuracy = []
# 1에서 10까지 n_neighbors를 적용
neighbors_settings = range(1,11)

for n_neighbors in neighbors_settings:
    # 모델 생성
    clf = KNeighborsClassifier(n_neighbors=n_neighbors)
    clf.fit(X_train, y_train)
    # 훈련 세트 정확도 저장
    training_accuracy.append(clf.score(X_train, y_train))
    test_accuracy.append(clf.score(X_test, y_test))
    
plt.plot(neighbors_settings, training_accuracy, label="훈련 정확도")
plt.plot(neighbors_settings, test_accuracy, label="테스트 정확도")
plt.ylabel("정확도")
plt.xlabel("n_neighbors")
plt.legend()
```




    <matplotlib.legend.Legend at 0xd2ca2e8>




![knn_png](https://degan85.github.io/assets/K-NN_19_1.png)


* 이웃을 하나 사용했을 때
    * 훈련 정확도는 높고 테스트 정확도는 낮다
    * 1-최근근접 이웃이 모델을 너무 복잡하게 만들었다는 설명
* 이웃을 10개 사용했을 때
    * 모델이 너무 단순해서 정확도는 더 나빠진다
* 정확도가 가장 좋을 때는 중간 정ㄷ인 여섯 개를 사용한 경우

## 최근접 이웃 회귀


```python
mglearn.plots.plot_knn_regression(n_neighbors=1)
```


![knn_png](https://degan85.github.io/assets/K-NN_22_0.png)



```python
mglearn.plots.plot_knn_regression(n_neighbors=3)
```


![knn_png](https://degan85.github.io/assets/K-NN_23_0.png)



```python
from sklearn.neighbors import KNeighborsRegressor
X, y = mglearn.datasets.make_wave(n_samples=40)

# wave 데이터셋을 훈련 세트와 테스트 세트로 나눕니다.
X_train, X_test, y_train, y_test = train_test_split(X, y, random_state=0)

# 이웃의 수를 3으로 하여 모델의 객체를 만듭니다.
reg = KNeighborsRegressor(n_neighbors=3)
# 훈련 데이터와 타깃을 사용하여 모델을 학습시킵니다.
reg.fit(X_train, y_train)

print("테스트 세트 예측: \n{}".format(reg.predict(X_test)))
```

    테스트 세트 예측: 
    [-0.05396539  0.35686046  1.13671923 -1.89415682 -1.13881398 -1.63113382
      0.35686046  0.91241374 -0.44680446 -1.13881398]
    


```python
# score 메서드를 사용해 모델 평가
print("테스트 세트 R^2: {:.2f}".format(reg.score(X_test, y_test)))
```

    테스트 세트 R^2: 0.83
    

## KNeighborsRegressor 분석


```python
fig, axes = plt.subplots(1, 3, figsize=(15,4))
# -3과 3사이에 1,000개의 데이터 포인트를 만듭니다.
line = np.linspace(-3, 3, 1000).reshape(-1, 1)
for n_neighbors, ax in zip([1,3,9], axes):
    # 1, 3, 9 이웃을 사용한 예측을 합니다.
    reg = KNeighborsRegressor(n_neighbors=n_neighbors)
    reg.fit(X_train, y_train)
    ax.plot(line, reg.predict(line))
    ax.plot(X_train, y_train, '^', c=mglearn.cm2(0), markersize=8)
    ax.plot(X_test, y_test, 'v', c=mglearn.cm2(1), markersize=8)
    
    ax.set_title("{} 이웃의 훈련 스코어: {:.2f} 테스트 스코어: {:.2f}".format(n_neighbors, reg.score(X_train, y_train), reg.score(X_test, y_test)))
    ax.set_xlabel("특성")
    ax.set_ylabel("타깃")
axes[0].legend(["모델 예측", "훈련 데이터/타깃", "테스트 데이터/타깃"], loc="best")
            
```








![knn_png](https://degan85.github.io/assets/K-NN_27_0.png)



```python
mglearn.cm2(1)
```




    (1.0, 0.12549019607843137, 0.12549019607843137, 1.0)



### 장단점과 매개변수
* 중요한 매개변수는 두 개
    데이터 포인트 사이의 거리를 재는 방법, 이웃의 수
* 보통 3개나 5개 정도
* 이해하기 쉬운 모델
* 훈련 세트가 매우 크면(특성의 수나 샘플의 수가 클경우) 예측이 느려짐
* 데이터 전처리가 중요
* 많은 특성을 가진 데이터셋에는 잘 동작하지 않음
* 특성 값 대부분이 0인(희소한) 데이터셋과는 잘 작동하지 않음


---

## Reference

[파이썬 라이브러리를 활용한 머신러닝](http://www.hanbit.co.kr/store/books/look.php?p_code=B6119391002)
