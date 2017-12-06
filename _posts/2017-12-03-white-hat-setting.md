---
layout: post
title:  "화이트해커 환경설정"
author: Degan
categories: white-hat 
tags:	white_hat hacker 
comments: true
---

## 순서
1. [VirtualBox 설치](#vb_install)
2. [Kali Linux 설치 및 설정](#ki_install)
3. [XAMPP 설치](#xampp_install)
4. [dvwa 설치](#dvwa_install)

---

<a name="vb_install"></a>
### 1. VirtualBox 설치

#### - 설치

![VirtualBox_install_1](https://degan85.github.io/assets/white-hat-setting-1.png)
![VirtualBox_install_2](https://degan85.github.io/assets/white-hat-setting-2.png)
* HOST 운영체제에 맞게 다운로드 
* usb 사용 등을 위해 All supported platforms도 같이 다운로드
* 둘 다 다운로드하고 설치를 진행

---

<a name="ki_install"></a>
### 2. Kali Linux 설치 및 설정

#### - 설치

![Kali_Linux_install_1](https://degan85.github.io/assets/white-hat-setting-3.png)
![Kali_Linux_install_2](https://degan85.github.io/assets/white-hat-setting-4.png)
![Kali_Linux_install_3](https://degan85.github.io/assets/white-hat-setting-5.png)

* **VirtualBox Images**로 다운로드
* 실행하면 VirtualBox가 열리고 가져오기 클릭
* 마우스 오른쪽을 누르면 시작, 정지, 설정 등을 할 수 있음
![Kali_Linux_install_4](https://degan85.github.io/assets/white-hat-setting-6.png)

* 설정을 누른 후 **시스템** 클릭
* 기본 메모리를 여유롭게(2GB정도) 선택
![Kali_Linux_install_5](https://degan85.github.io/assets/white-hat-setting-7.png)

* 시작하면 사용자 ID와 비밀번호를 입력해야함
* 초기 사용자 ID : root
* 초기 비밀번호 : toor

#### - 한글 폰트 설치
* 터미널을 열고 `apt-get install -y fonts-nanum` 입력
* 완료되면 `fc-cache -f -v`를 입력

* **settings**에서 **Resion & Language** -> 맨 아래 **한국어** 설정 -> 다시 시작

#### - 한영 변환
##### 터미널에서 
* `apt-get install -y nabi` 실행 
* `apt-get install -y im-config`을 실행 
* `im-config` 실행 -> 확인 -> yes -> **hungul** 선택 -> 확인 -> 로그아웃
* `shift` + `space`로 한영 변환 

![Kali_Linux_install_4](https://degan85.github.io/assets/white-hat-setting-8.png)

#### - 스냅샷 복원지점
* 스냅샷 -> 찍기 
* 스냅샷을 찍어 복원지점을 생성
![Kali_Linux_install_5](https://degan85.github.io/assets/white-hat-setting-9.png)
 
* 복원 시 마우스 오른쪽 '복원' 선택
![Kali_Linux_install_6](https://degan85.github.io/assets/white-hat-setting-10.png)

* 문제가 발생 할 경우 복원지점으로 복원할 수 있음

---

<a name="xampp_install"></a>
### 3. XAMPP 설치

#### - 설치
![XAMPP_install_1](https://degan85.github.io/assets/white-hat-setting-11.png)

* DVWA 지원을 위해 **5**점대 사용
![XAMPP_install_2](https://degan85.github.io/assets/white-hat-setting-12.png)

* 터미널에서 `cd Downloads/` 입력
* `chmod +x ./xampp-linux-5.6.23-0-installer.run`를 입력해서 권한을 줌

#### - 실행(터미널)
* `ls -al opt/xampp`
* `gedit /opt/lampp/etc/php.ini`
* ctrl + f(검색) -> **allow_url_include** -> **On**으로 변경
* XAMPP 어플리케이션을 열어서 MySQL Database start & Apache Web Server restart

---

<a name="dvwa_install"></a>
### 4. DVWA 설치

* 브라우저에서 http://localhost/dashboard/ 접속
* phpMyAdmin 클릭
![DVWA_install_1](https://degan85.github.io/assets/white-hat-setting-13.png)


* db에 dvwa 데이터베이스 만들기
![DVWA_install_2](https://degan85.github.io/assets/white-hat-setting-14.png)

* 브라우저에서 dvwa github 검색(https://github.com/ethicalhack3r/DVWA/releases) -> **zip파일 다운로드**
* 압축 풀고 브라우저에서 접속 할 수 있게 폴더 이동(터미널에서 `mv DVWA-1.9 /opt/lampp/htdocs/dvwa` 입력)
* 터미널에서 `sudo chmod -R 0777 /opt/lampp/htdocs/` 실행
* 브라우저에서 localhost/dvwa 이동.(빨간색인 부분을 녹색으로 변경해야 함)
* https://www.google.com/recaptcha/admin 접속
* label에 **dvwa** 입력 -> domain에 'localhost' -> KEY 생성
* 터미널에서 `cd /opt/lampp/htdocs/dvwa/` 실행 - > `gedit config/config.inc.php` 실행
* reCAPTCHA 검색 -> public_key와 private_key 입력 -> 위에 db_password값 삭제(기본값)
* **Create / Reset Database** 클릭

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
