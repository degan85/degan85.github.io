---
layout: post
title:  "Git ssh key"
author: Degan
categories: git 
tags:	git ssh rsa
comments: true
---

# github ssh key

## 1. ssh key 생성

```bash
ssh-keygen -t rsa -b 4096 -C <"내꺼email">
```

## 2. github에 key 등록
    - 오른쪽위 내정보 클릭
    - Settings
    - SSH and GPG keys
    - New SSH key 
    - 생성된 공개키 등록(*.pub)

## 3. ssh 접속중이라 에러 발생하면

```bash
ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
```

## 4. git clone
- git repository > Code > SSH 복사

```bash
git clone git@github.com:....
```


