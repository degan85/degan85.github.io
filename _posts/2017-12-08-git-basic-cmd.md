---
layout: post
title:  "Git 기초 명령어 정리"
author: Degan
categories:  
tags:	git init status add merge commit ignore
comments: true
---

## 목차
1. [init](#init)
2. [ignore 파일 설정](#ignore)
3. [status](#status)
4. [add](#add)
5. [commit](#commit)
6. [log](#log)
7. [좋은 commit 메시지](#commit_meg)
8. [diff](#diff)
9. [branch](#branch)
10. [checkout](#checkout)
11. [merge](#merge)

---

<a name="init"></a>
## init
- `git init`
- 현재 디렉토리를 Git 레포지토리로 설정하는 명령
- .git 디렉토리가 생성 됨
- 기본적으로 master로 branch가 할당 됨
- [Git의 기초 - Git 저장소 만들기](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-Git-%EC%A0%80%EC%9E%A5%EC%86%8C-%EB%A7%8C%EB%93%A4%EA%B8%B0)

---

<a name="ignore"></a>
## ignore 파일 설정
- 설정한 파일들을 git 저장소에 저장하지 않게 관리
	* os에서 생성하는 파일, log 파일 등 불필요한 파일
	* 개인정보, api key 같은 보안과 관련이 있는 파일
	* 환경설정 파일
- .gitignore 파일 생성
```bash
.DS_Store
*.log
*.iml
tem/
```
- 언어, 프로젝트 마다 자동 생성파일들이 다름.
- 확인해서 init 후 ignore 파일을 설정해주는 것이 좋음
- [.gitignore sample](https://gist.github.com/octocat/9257657), [JAVA .gitignore sample](https://gist.github.com/games647/796e21a661a7a96b1e67d78fa2e1257f), [Android Studio .gitignore sample](https://gist.github.com/iainconnor/8605514) 

---

<a name="status"></a>
## status
- git 저장소의 상태를 보여주는 명령어
- 처음에 생성 후 `git status`를 실행하면 
- **nothing to commit** (커밋 할 것이 없다고 나옴)


- 새로운 파일을 생성하고 `git status`를 실행
-  파일을 staging area로(탐지하기 위해) add 하는 방법이 써져있음

```bash
Untracked files:
  (use "git add <file>..." to include in what will be committed)

        index.html
```

- `git add index.html`을 진행한 후 `git status`를 실행
- staging area에서 제거하기 위해 사용하는 명령이 써져있음

```bash
Changes to be committed:
  (use "git rm --cached <file>..." to unstage)

        new file:   index.html
```

---

<a name="add"></a>
## add
- Working Directory의 파일을 Staging Area로 옮기는 명령

```bash
git add <파일명>
git add .
git add -i
```

- 모두 한번에 add하기 보다 각각의 파일을 올려 주는 것이 좋음 
- `git add -i`를 입력하면 명령 프롬프트가 나옴
- 4번(**add untracked**)을 누르고 원하는 파일만 번호를 눌러 추가
- 종료하려면 엔터를 한번 더 누르고 `q`를 입력 

```bash
$ git add -i

*** Commands ***
  1: status       2: update       3: revert       4: add untracked
  5: patch        6: diff         7: quit         8: help
What now>
```

---

<a name="commit"></a>
## commit
- Staging Area에 있는 파일을 Repository에 저장하는 명령

- `git commit`을 입력하면 에디터가 출력되고 메시지를 입력하게 함
- 저장하지 않으면 커밋이 취소 됨
- `git commit -m [커밋 메시지]`를 입력하면 터미널에서 바로 메시지 입력 후 커밋 됨
- 한번 Repository에 올린 파일이 다시 수정되면 add부터 다시 해야 함
- `git commit -a -m [커밋 메시지]`를 입력하면 자동으로 해당 파일들을 add하고 커밋함

```bash
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   index.html
```

- [Git의 기초 - 수정하고 저장소에 저장하기](https://git-scm.com/book/ko/v1/Git%EC%9D%98-%EA%B8%B0%EC%B4%88-%EC%88%98%EC%A0%95%ED%95%98%EA%B3%A0-%EC%A0%80%EC%9E%A5%EC%86%8C%EC%97%90-%EC%A0%80%EC%9E%A5%ED%95%98%EA%B8%B0)

---

<a name="log"></a>
## log
- commit 이력을 조회
- `git log`	: 상세하게 commit 이력 조회
- commit ID, 작성자, 시간, 타이틀, 세부 메시지 모두 조회 됨

```bash
commit 486aae7bd415516587e9b50a788e3e414c1a101c (HEAD -> master)
Author: cho <degan8535@gmail.com>
Date:   Fri Dec 8 09:17:17 2017 +0900

    Modify html tag

    - Add html tag

```

- `git log --oneline` : commit ID, 타이틀 메시지만 조회

```bash
486aae7 (HEAD -> master) Modify html tag
```

- 'git log --online --decorate --graph --all` : 모든 브랜치 commit 이력 조회
- `git log -- index.html` : 특정 파일의 변경 commit 조회 

```bash
commit bd547430c789a00f1db4e132ed29e8fceefb0e41
Author: cho <degan8535@gmail.com>
Date:   Fri Dec 8 09:14:25 2017 +0900

    Add html tag

commit 52c10148aa4b1f408348428496a994aec8773fed
Author: cho <degan8535@gmail.com>
Date:   Fri Dec 8 09:12:35 2017 +0900

    Add index.html
```

---

<a name="commit_meg"></a>
## 좋은 commit 메시지
- 간단하고 명확하게
- title은 변경에 대한 짧은 요약(50자 이내)
- 필요한 경우 상세설명(한줄에 72자 이내)
- 추가적인 문단은 '-'를 붙인다
- 영문은 첫글자는 대문자, 현재형, 동사로 시작

---

<a name="diff"></a>
## diff
- 다른 commit과 Working 디렉토리를 비교하는 명령
- `git diff` : 현재 브랜치의 마지막 commit과 차이점 비교
- `git diff [Commit ID]` : 특정 commit과의 차이점 비교
- `git diff [Commit ID] -- [파일경로]` : 특정 commit과 특정 파일의 차이점 비교

---

<a name="branch"></a>
## branch
- 브랜치를 생성, 수정, 삭제등을 명령
- `git init`을 하면 브랜치명(master)이 생성 됨

```bash
~/git_branch
$ git init
Initialized empty Git repository in C:/Users/사용자이름/git_branch/.git/

~/git_branch (master)
```

- `git branch [브랜치명]` : 브랜치 생성
- `git branch -d [브랜치명]` : 브랜치 삭제
- `git branch -m [이전 브랜치명] [변경할 브랜치명]` : 브랜치 이름변경

---

<a name="checkout"></a>
## checkout
- Working 디렉토리의 소스를 특정 commit으로 변경
- `git checkout [브랜치명]` : 특정 브랜치로 워킹 디렉토리 변경
- `git checkout [Commit ID]` : 특정 commit으로 워킹 디렉토리 변경
- `git checkout -- [파일 경로]` : 특정 파일을 해당 브랜치 또는 commit 상태로 변경

- develop 브랜치에서 index.html에 tag를 추가하고 commit 함
- log를 확인하면 release와 master 브랜치는 첫 번째 커밋만 되있음
- develop 브랜치는 3407eac값의 두 번째 커밋도 돼있음

```bash
~/git_checkout (develop)
$ git log --graph --oneline --decorate --all
* 3407eac (HEAD -> develop) Add tag
* 97ee0bf (release, master) Add index.html
```

- develop 브랜치에서 index.html파일과 master, release 브랜치의 index.html 파일이 달라짐

```bash
~/git_checkout (develop)
$ cat index.html
<html> </html>

~/git_checkout (develop)
$ git checkout master
Switched to branch 'master'

~/git_checkout (master)
$ cat index.html
```

- release 브랜치에서 about.html 파일을 생성 후 commit 하면 
- release 브랜치에서만 about.html 파일이 보임
- master 브랜치로 checkout 하면 about.html이 사라짐

```bash
~/git_checkout (release)
$ ls
about.html  index.html

~/git_checkout (release)
$ git checkout master
Switched to branch 'master'

~/git_checkout (master)
$ ls
index.html
```

- `git log --oneline --graph --all` 입력
- 그래프상 release 아래에 있는 develop, master에서는 about.html이 보이지 않음
- develop 브랜치는 index.html을 수정해서 가지가 생겨서 나옴

```bash
$ git log --oneline --all --graph
* 8a70473 (release) Add about.html
| * 3407eac (develop) Add tag
|/
* 97ee0bf (HEAD -> master) Add index.html
```

---
checkout에 따라 파일 정보가 바뀜. 항상 자신의 브랜치를 확인하고 수정 및 commit을 진행해야 함.

- `git checkout [되돌리고 싶은 commit ID] -- [파일 이름]`
- 잘못한 commit이 있을경우 전 상황으로 복구
- index.html을 **commit ID 97ee0bf** 상태로 돌림

```bash
~/git_checkout (master)
$ git log --oneline
588c263 (HEAD -> master) Add html tag
97ee0bf Add index.html

~/git_checkout (master)
$ cat index.html
<html>

</html>

~/git_checkout (master)
$ git checkout 97ee0bf -- index.html

~/git_checkout (master)
$ cat index.html

```

---

<a name="merge"></a>
## merge
- 다른 두개의 소스를 병합하는 명령 
- 중복되는 위치를 변경하지 않는 경우 merge가 잘 됨
- 같은 파일의 같은 부분의 내용이 변경되면 잘 안 됨(CONFLICT)

```bash
$ git merge develop
Auto-merging sum_of_1_to_100.js
CONFLICT (content): Merge conflict in sum_of_1_to_100.js
Automatic merge failed; fix conflicts and then commit the result.
```

- 상태를 보면 어느 파일이 충돌 났는지 나옴

```bash
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   sum_of_1_to_100.js

no changes added to commit (use "git add" and/or "git commit -a")
```

- 파일을 확인하면 CONFLICT(충돌) 난 부분이 다음과 같이 표시

```javascript
<<<<<<< HEAD
for (var i=1; i<101; i++) {
=======
for (var i=0; i<100; i++) {
>>>>>>> develop
```

- 맞는 부분으로 코드를 수정하고 다시 commit

```bash
$ git log --decorate --graph --all
*   commit dbe1ad750752ed27e9e98a64f1d076686e13a381 (HEAD -> master)
|\  Merge: e858b63 5f19d83
| | Author: cho <degan8535@gmail.com>
| | Date:   Fri Dec 8 14:04:31 2017 +0900
*   commit dbe1ad750752ed27e9e98a64f1d076686e13a381 (HEAD -> master)
| |
| |     Merge develop
| |
| * commit 5f19d83e0dcc854afb9eb10c0dcabd25bfbb075d (develop)
| | Author: cho <degan8535@gmail.com>
| | Date:   Fri Dec 8 13:54:20 2017 +0900
| |
| |     Modify for statement
| |
* | commit e858b6399ce0cec1ef43c93a07ddc1ec7b7454d3
|/  Author: cho <degan8535@gmail.com>
|   Date:   Fri Dec 8 13:55:49 2017 +0900
|
|       Modify 101
|
* commit 556bda83a3a935a91e0f83b0fce623fb19612486
  Author: cho <degan8535@gmail.com>
  Date:   Fri Dec 8 13:52:19 2017 +0900

      Add sum of
```

---

## Reference

[누구나 쉽게 배우는 Git](https://www.udemy.com/how_to_start_git_and_github/learn/v4/overview)
