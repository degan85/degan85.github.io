---
layout: post
title:  "Git ssh key"
author: Degan
categories: git 
tags:	git ssh rsa
comments: true
---

# github ssh key

## ssh key 생성

```bash
ssh-keygen -t rsa -b 4096 -C <"내꺼email">
```

## github에 key 등록
    - 오른쪽위 내정보 클릭
    - Settings
    - SSH and GPG keys
    - New SSH key 
    - 생성된 공개키 등록(*.pub)

## ssh 접속중이라 에러 발생하면

```bash
ssh-keyscan -t rsa github.com >> ~/.ssh/known_hosts
```

## git clone
- git repository > Code > SSH 복사

```bash
git clone git@github.com:....
```

## 확인

```bash
ssh -T git@github.com
# You've successfully authenticated, ~
```

## 계정 여러개 등록
- 위 방법으로 keygen 생성
  - 다른 이름으로(ex id_rsa2)
- ~/.ssh 경로에 config 파일 생성

```bash
Host github.com
	HostName github.com
	User cho
	IdentityFile ~/.ssh/id_rsa
Host gitlab.com
	HostName github.com
	User cho2
	IdentityFile ~/.ssh/id_rsa2
```

