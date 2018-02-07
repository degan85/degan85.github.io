---
layout: post
title: " How to add an HTML Editor for Eclipse "
author: Degan
categories: problem
tags:	eclipse html editor oxygen
comments: true
---

## 발생
- **Windows** -> **perspective** 설정을 바꾸다가(SDK) 
- html을 열어보니 browser로만 열리고 소스코드 수정을 못 함

## 해결
- **Help** -> **Install New Software...**
- **Work with** 리스트를 내림(▼)
-  **Oxygen** (http://download.eclipse.org/releases/oxygen) 선택
- oxygen eclipse 기준
- Web,XML,Java EE and ... 선택
- 인스톨하고 재부팅
- Window -> Preferences
- **General** -> **Editors** -> **File Associations**
- *.html 선택
- HTML Edotor 보임

---

## Reference


[How to add an HTML Editor for Eclipse](http://lizusefulstuff.blogspot.kr/2012/07/how-to-add-html-editor-for-eclipse.html)
