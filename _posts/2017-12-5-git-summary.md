---
layout: post
title:  "Git 정리"
author: Degan
categories: git 
tags:	git config section  
comments: true
---

## 목차
1. [History](#history)
2. [Git vs SVN 트렌드 비교](#git_vs_svn)
3. [장점](#advantage)
4. [branch 전략(Gitflow)](#gitflow)
5. [동작원리](#principle)
6. [설정 파일 정보 확인](#config)

---

<a name="history"></a>
## 1. History
리눅스 토발즈가 리눅스 커널 프로젝트를 위해 버전 관리 시스템으로 개발(2005년)

---

<a name="git_vs_svn"></a>
## 2. Git vs SVN 트렌드 비교
<script type="text/javascript" src="https://ssl.gstatic.com/trends_nrtr/1225_RC05/embed_loader.js"></script> <script type="text/javascript"> trends.embed.renderExploreWidget("TIMESERIES", {"comparisonItem":[{"keyword":"git","geo":"","time":"today 5-y"},{"keyword":"svn","geo":"","time":"today 5-y"}],"category":0,"property":""}, {"exploreQuery":"date=today 5-y,today 5-y&q=git,svn","guestPath":"https://trends.google.com:443/trends/embed/"}); </script> 

---

<a name="advantage"></a>
## 3. 장점
- 속도
	* 네트워크: 분산형 (로컬에서 관리)
	* 파일 처리 : 해시 알고리즘, 파일 변경 여부 판단, 변경했으면 해당 링크만
- 동시 다발적인 개발
- 책임성
- 대형 프로젝트를 효율적으로 지원

---

<a name="gitflow"></a>
## 4. branch 전략(Gitflow)
- Master(언제든지 릴리즈 가능한 상태 유지)
- Hotfix(Master에서 버그가 발견 됐을때 즉각 대응시 필요. 수정 후 다시 Master랑 Develop에 merge)
- Release(develop에서 Master로 올리기 전에, Master나 Develop에 merge)
- Develop(개발 작업)
- Feature(develop에서 파생, 여러개 작업, develop에 merge)

![gitflow](https://media.licdn.com/mpr/mpr/AAEAAQAAAAAAAAywAAAAJDkyMTNlNTY1LTliY2MtNDYyMy1hNGM4LWY4ZGI1NmFhN2NiNQ.png)
출처:[GitFlow: A Better Branching Strategy for Your Projects - Introduction to the Branching Model](https://www.linkedin.com/pulse/gitflow-better-branching-strategy-your-projects-model-kodagoda)

---

<a name="principle"></a>
## 5. 동작 원리

	- snapshot
		* 데이터를 가져오거나 프로젝트를 저장할 때마다 그 시점의 파일에 대해 스냅샷을 저장
		* 변경되지 않은 파일은 저장하지 않고 이전에 지정한 동일한 파일을 링크
	- checksum
		* 데이터를 저장하기 전에 체크섬을 구함
		* 이 체크섬을 통해 데이터를 관리
		* SHA-1 해쉬 사용(16진수 문자 40개로 구성된 문자열)
		* 파일 이름이 아닌 컨텐츠의 해쉬 값을 저장
		* 파일명이 변경되도 내용이 동일하면 같은 해쉬
	- sections(영역)
		* Working Directory(저장소에서 클론을 생성)
		* Staging Area(내가 만든 파일 add)
		* Repository(staging area에서 저장소로 커밋)

---

<a name="config"></a>
## 6. 설정 파일 정보 확인
	- 책임성을 유지하기 위해 commit한 사람의 email과 이름이 반드시 필요함.
	- USER_HOME/.gitconfig
	- Working Directory/XXX/.git/config(우선순위가 더 높음)
	- .gitconfig 파일일 없을 경우 사용자 email과 이름 등록해서 사용함
		ex) `git config --global user.name username`, `git config --global user.email user@email.com`
		
---

## Reference

[누구나 쉽게 배우는 Git](https://www.udemy.com/how_to_start_git_and_github/learn/v4/overview)
