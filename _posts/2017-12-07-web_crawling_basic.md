---
layout: post
title: "web crawling basic"
author: Degan
categories: webcrawling
tags:	jupyter python notebook crawling data
original_extension: ipynb
comments: true
---
## 1.1 연결

### 브라우저로 정보를 가져오는 구조

밥은 데스크톱 컴퓨터를 가지고 있고 앨리스의 서버에 연결하려고 함

1. 밥의 컴퓨터는 1과 0으로 된 비트 스트림을 보냄.
    - 각 비트는 전압으로 구별.
    - 이들 비트는 정보를 구성하며, 헤더와 바디도 그런 정보에 포함
    - 헤더에는 바로 다음 목표인 밥의 라우터 MAC 주소와 최종 목표인 앨리스 IP주소가 들어있음
    - 바디에는 밥이 앨리스의 서버 애플리케이션에 요청하는 내용이 있음

2. 밥의 라우터는 이들 비트를 받아 인터넷에 보냄
    - 밥의 MAC 주소에서 앨리스의 IP 주소로 가는 패킷을 해석
    - 밥의 라우터의 고유 IP 주소를 패킷에 '발신자 from' 주소로 기록

3. 밥의 패킷은 여러 중간 서버를 거치며 이동
    - 중간 서버들은 정확한 물리적 경로 또는 유선 경로를 거쳐 앨리스의 서버를 향해 패킷을 보냄

4. 앨리스의 서버는 자신의 IP 주소에서 그 패킷을 받음

5. 앨리스의 서버는 패킷 헤더에서 포트번호를 찾고 적절한 애플리케이션(웹 서버 애플리케이션)에 보냄
    - 웹 애플리케이션의 포트는 거의 80
    - IP 주소가 거리 주소라면 포트 번호는 패킷 데이터의 아파트 동수?

6. 웹 서버 애플리케이션은 서버 프로세서에서 데이터 스트림을 받음
    - 다음과 같은 정보가 들어있음
    - 이 요청은 GET 요청임
    - 요청하는 파일은 index.html임

7. 웹 서버는 해당 HTML 파일을 찾고 새 패킷으로 묶어 자신의 라우터에 보냄
    - 라우터는 밥의 컴퓨터로 전송
    - 웹 서버가 보낸 패킷은 밥이 보낸 패킷과 같은 과정으로 밥의 컴퓨터에 도달

---
#### 웹 브라우저는 이들 패킷을 만들고, 보내고, 돌아온 데이터를 해석해 표현하는 애플리케이션.
##### 파이썬에서도 처리 가능


In[
1
]
```python
# urlopen은 네트워크를 통해 원격 객체를 읽음

from urllib.request import urlopen
html = urlopen("http://pythonscraping.com/pages/page1.html")
print("전송 받은 HTML 코드 : ")
print(html.read())
```

    전송 받은 HTML 코드 : 
    b'<html>\n<head>\n<title>A Useful Page</title>\n</head>\n<body>\n<h1>An Interesting Title</h1>\n<div>\nLorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.\n</div>\n</body>\n</html>\n'
    

## 1.2 BeautifulSoup
잘못된 HTML을 수정하여 쉽게 탐색할 수 있는 XML 형식의 파이썬 객체로 변환


In[
2
]
```python
from bs4 import BeautifulSoup
html = urlopen("http://pythonscraping.com/pages/page1.html")
bsObj = BeautifulSoup(html.read(), "html.parser")

print(bsObj)
```

    <html>
    <head>
    <title>A Useful Page</title>
    </head>
    <body>
    <h1>An Interesting Title</h1>
    <div>
    Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.
    </div>
    </body>
    </html>
    
    


In[
3
]
```python
# 셋 다 동일
print(bsObj.html.body.h1)
print(bsObj.body.h1)
print(bsObj.h1)
```

    <h1>An Interesting Title</h1>
    <h1>An Interesting Title</h1>
    <h1>An Interesting Title</h1>
    

### 신뢰할 수 있는 연결 (예외처리)


In[
4
]
```python
from urllib.error import  HTTPError

def getTitle(url):
    try:
        html = urlopen(url)
    # 400, 500 error
    except HTTPError as e:
        return None
    try:
        bsObj = BeautifulSoup(html.read(), "html.parser")
        title = bsObj.body.h1
    # 반환된 객체가 없는 경우
    except AttributeError as e:
        return None
    return title

title = getTitle("http://www.pythonscraping.com/pages/page1.html")
if title == None:
    print("Title could not be found!")
else:
    print(title)
```

    <h1>An Interesting Title</h1>
    

### 스타일시트 사용
* CSS의 등장은 웹 스크레이퍼에도 큰 도움이 됨
* 클래스를 이용해 쉽게 태그들을 구별

### .get_text()
* 현재 문서에서 모든 태그를 제거하고 문자열만 반환
* 텍스트 블록보다는 BeautifulSoup 객체를 사용하는게 더 유용
* 그래서 최종 데이터를 출력하거나 저장, 조작 직전에만 사용

### find()와 findAll()
* find(tag, attributes, recusive, text, keywords)
* findAll(tag, attributes, recusive, text, limit, keywords)
* 실제로 두 매개변수(tag, attributes)만 거의 사용


* tag :
    * 태그 문자열 이름이나 리스트로 넘김
    * `.findAll({"h1", "h2", "h3", "h4"})`
* attributes :
    * 속성으로 이루어진 파이썬 딕셔너리를 받고 그중 하나 일치하는 태그 찾음
    * .findAll("span",{"class":{"green","red"}})
* recursive :
    * boolean 값임
    * True면 일치하는 태그를 찾아 자식, 자식의 자식 검색
    * false면 문서의 최상위 태그만 검색
    * 재귀적으로 True로 설정돼있음
* text :
    * 태그 속성이 아니라 텍스트 콘텐츠에 일치
    * `.findAll(text="the prince")`
* limit :
    * findAll에서 limit를 1로 지정하면 find와 같음



In[
5
]
```python
html = urlopen("http://www.pythonscraping.com/pages/warandpeace.html")
bsObj = BeautifulSoup(html, "html.parser")

nameList1 = bsObj.findAll("span",{"class":"green"},limit=1)
for name in nameList1:
    print(name.get_text())
```

    Anna
    Pavlovna Scherer
    


In[
6
]
```python
nameList2 = bsObj.find("span",{"class":"green"})
print(nameList2.get_text())
```

    Anna
    Pavlovna Scherer
    


In[
7
]
```python
nameList3 = bsObj.findAll(text="the prince")
print(len(nameList3))
```

    7
    

### 트리 이동
* 문서 안에서의 위치를 기준으로 태그 검색
* .children : 자식만 검색
* .descendants : 자식에 포함된 태그(img, span 등등) 모두 포함되서 검색
* .next_siblings, previous_siblings : 객체 자신을 제외한 형제들을 반환
* .next_sibling, previous_sibling : 태그 하나만 반환
* .parent : 부모 검색
* 페이지 레이아웃은 시시때때 변함. 따라서 가능하다면 태그 속성을 활용


In[
8
]
```python
html = urlopen("http://www.pythonscraping.com/pages/page3.html")
bsObj = BeautifulSoup(html, "html.parser")

print(bsObj.find("img",{"src":"../img/gifts/img1.jpg"}).parent.previous_sibling.get_text())
```

    
    $15.00
    
    

### 정규 표현식



In[
9
]
```python
import re
html = urlopen("http://www.pythonscraping.com/pages/page3.html")
bsObj = BeautifulSoup(html, "html.parser")
images = bsObj.findAll("img", {"src": re.compile("\.\.\/img\/gifts\/img.*\.jpg")})

for image in images:
    print(image)
```

    <img src="../img/gifts/img1.jpg"/>
    <img src="../img/gifts/img2.jpg"/>
    <img src="../img/gifts/img3.jpg"/>
    <img src="../img/gifts/img4.jpg"/>
    <img src="../img/gifts/img6.jpg"/>
    

### 속성에 접근하기


In[
10
]
```python
for image in images:
    print(image["src"])
```

    ../img/gifts/img1.jpg
    ../img/gifts/img2.jpg
    ../img/gifts/img3.jpg
    ../img/gifts/img4.jpg
    ../img/gifts/img6.jpg
    

### 람다 표현식
* 간단히 말해 다른 함수에 변수로 전달되는 함수
* 태그 객체를 매개변수로 받아야 함
* boolean만 반환 할 수 있음
* BeautifulSoup는 모든 객체를 이 함수에서 평가하고, true로 평가된 태그만 반환
* `soup.findAll(lambda tag:len(tag.attrs) == 2)`

---

## Reference

[파이썬으로 웹 크롤러 만들기](http://www.hanbit.co.kr/store/books/look.php?p_code=B7159663510)
