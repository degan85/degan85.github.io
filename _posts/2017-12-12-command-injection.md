---
layout: post
title:  "Github basic"
author: Degan
categories:  git
tags:	github push pull fetch remote clone request
comments: true
---

## Command Injection 공격

- 웹을 통해 시스템 명령어를 실행하는 공격
- 웹 내부에서 **시스템 명령어**를 실행하는 경우, 입력값을 제대로 검사하지 않으면, 해커 마음대로 시스템 명령어를 실행

### 실습 환경

![brute-force](https://degan85.github.io/assets/command-injection-1.png)

- IP만을 입력했을 경우 정상 작동

![brute-force](https://degan85.github.io/assets/command-injection-2.png)

- cmd에서 `ping`명령어 실행 결과와 동일

![brute-force](https://degan85.github.io/assets/command-injection-4.png)

### Low 단계

- `shell_exec()`를 통해 단순히 시스템 명령어를 실행

![brute-force](https://degan85.github.io/assets/command-injection-3.png)

- 리눅스의 경우 `;`뒤에 다른 명령어를 입력하면 시스템 명령어가 계속 실행 됨

![brute-force](https://degan85.github.io/assets/command-injection-6.png)

- `cat /etc/passwd`같이 중요 정보를 요청하면 위험

### Medium 단계

- `;`(리눅스)과 `&&`는 막았지만 `|` 또는 `&`도 막아야함

![brute-force](https://degan85.github.io/assets/command-injection-7.png)

### High 단계

- 불필요한 입력 값들을 제거해줘야 함

![brute-force](https://degan85.github.io/assets/command-injection-8.png)

- DVWA의 경우 개발자가 `|`다음에 공백을 준 것만 제거를 해서 공백을 붙이지 않고 `|<명령어>`를 입력하면 실행 됨

![brute-force](https://degan85.github.io/assets/command-injection-9.png)

### Impossible
- 필요한 입력 값만 검증
- **high**단계와 같이 불필요한 것들을 모두 제거

![brute-force](https://degan85.github.io/assets/command-injection-10.png)

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
