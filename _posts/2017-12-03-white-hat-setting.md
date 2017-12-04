---
layout: post
title:  "화이트해커 환경설정"
author: Degan
categories: white-hat 
tags:	white_hat hacker 
comments: true
---

## 순서
1. VirtualBox 설치
2. Kali Linux 설치 및 설정
3. XAMPP 설치
4. dvwa 설치

---

### 1. VirtualBox 설치

#### - 설치

![VirtualBox_install_1](https://lh3.googleusercontent.com/EIsNBtj_wvBNhPCRZEECl6yzuFI8PDbRfFb348d0c8As_sfX9XVtCfJPFCrjjyOu3IroPqiLYkjjFpKBlnTznNVbhuu2L3mCxocVtzDVQamosPKYTBV7WTMHVuYwqcokeiK5eDMM3BlC4sCrdXrB-dWjTopbbr1RY5RnsMqy4R3Qc4ye3afTRZJFzB86qvuSt38ortN0dryObOzFr--zTsb2u_aPK3VVqXtJ2Abz6IG-g-HvVWR2zT9xuC0A3nlHIhwU4EDrm-znsl8ijEbjinTmRfATSSkVo2l6gJTVztrL5muR13of_Km4tOWGjflvlxvgMtHG8TjMKSJeL8anvAXOGolVE_X3kJlqfZO6C9WJVueQAF9p5NOadT24eErw_WtjSaUiKFT2zBPnjnvzKBcRTvGV4Jc0sJ6SXDTxQbXEEqmHS8zwYuFlfT7SdqrMTFc7JO-cRP9cnVzeUcHhSKiMlAZG-ubIoFnGsfI01STV99u-zEbc0vtA1tg11DLGuBnVm-wsachuCdJN6D_XuEHe24tqRwj0-xm_rVe4BMhHv9qCaAPHnExCLz56aBPEEv_4vS7upJehnFQKZ6zlsdsaX81yUjnzGax1uHnXlA=w944-h423-no)
![VirtualBox_install_2](https://photos.google.com/photo/AF1QipOZCVAt7CmdL3rzMgVxyBJUHMzquLF2dTFXI7eg)
HOST 운영체제에 맞게 다운로드하고 usb 사용 등을 위해 All supported platforms도 같이 다운로드합니다.
둘 다 다운로드하고 설치를 진행합니다.

---
#### 2. Kali Linux 설치 및 설정

#### - 설치

![Kali_Linux_install_1](https://photos.google.com/photo/AF1QipO55o5t7yeTDr32sbidKX8cgBjrSjAECz9hQrYc)
![Kali_Linux_install_2](https://photos.google.com/photo/AF1QipMG2VhqguOnDkr_ATVlTSlD7tW4ZJdQUGxn4-uw)
![Kali_Linux_install_3](https://photos.google.com/photo/AF1QipO7iCJSrnIjd6MEi_svfPe5PXn-jq2REhgh_d7W)

VirtualBox Images로 다운로드합니다.
실행하면 VirtualBox가 열리고 가져오기를 합니다.

![Kali_Linux_install_4](https://photos.google.com/photo/AF1QipOoN4IXrKjK8JzUTBrTKnY4uhbYOlWuxMIIh5AS)
마우스 오른쪽을 누르면 시작, 정지, 설정 등을 할 수 있습니다.

![Kali_Linux_install_5](https://photos.google.com/photo/AF1QipNzziJbEax8aHB8eNda9ZJ9sU6CUPOU-GVJ157L)
설정을 누른 후 **시스템**을 누릅니다. 기본 메모리를 여유롭게 잡습니다.(2GB정도)

시작하면 사용자 ID와 비밀번호를 입력해야합니다.
초기 사용자 ID : root
초기 비밀번호 : toor

![Kali_Linux_install_4](https://photos.google.com/photo/AF1QipM4lP8bqSYxniGSRNfE8qH2pvxo3VG83fdPlTjZ)

#### - 한글 폰트 설치
터미널을 열고 `apt-get install -y fonts-nanum1` 입력합니다.
완료되면 `fc-cache -f -v`를 입력합니다.

**settings**에서 **Resion & Language** -> 맨 아래 **한국어** 설정 -> 다시 시작

#### - 한영 변환
터미널에서 
`apt-get install -y nabi` 실행합니다. 
`apt-get install -y im-config`을 실행합니다. 
`im-config` 실행 -> 확인 -> yes -> **hungul** 선택 -> 확인 -> 로그아웃
`shift` + `space`로 한영 변환됩니다. 

#### - 스냅샷 복원지점
![Kali_Linux_install_5](https://photos.google.com/photo/AF1QipMY3RUzSCERE7JzGUeDtNUCvV_q8m4msi1lfJdr)
(스냅샷 -> 찍기) 
스냅샷을 찍어 복원지점을 만듭니다. 

![Kali_Linux_install_6](https://photos.google.com/photo/AF1QipNu9ZnZEDSH2SIXoN6BYVvI2zSg4ordFjpayP5d)
(복원 시 마우스 오른쪽 '복원' 선택)
문제가 발생 할 경우 복원지점으로 복원할 수 있습니다.

---

### 3. XAMPP 설치

#### - 설치
![XAMPP_install_1](https://photos.google.com/photo/AF1QipOMBZrUx8wDp-TaEj_FX4rac0NkE3qfGFQtGKzu)
![XAMPP_install_2](https://photos.google.com/photo/AF1QipPF16AhDSfygt1kUZJJgn9SnO9eaYOu3Q73R0vB)
(DVWA 지원을 위해 5점대 사용)

터미널에서 `cd Downloads/` 입력합니다.
`chmod +x ./xampp-linux-5.6.23-0-installer.run`를 입력해서 권한을 줍니다.

#### - 실행(터미널)
* `ls -al opt/xampp`
* `gedit /opt/lampp/etc/php.ini`
* ctrl + f(검색) -> **allow_url_include** -> **On**으로 변경
* XAMPP 어플리케이션을 열어서 MySQL Database start & Apache Web Server restart

---

### 4. DVWA 설치

브라우저에서 http://localhost/dashboard/ 접속
![DVWA_install_1](https://photos.google.com/photo/AF1QipOTaRXN-uBTP3rPSsp5dJWanisbr0r0Phj5qb99)
(phpMyAdmin 클릭)

![DVWA_install_2](https://photos.google.com/photo/AF1QipMWmwsnW97Q5XBVzOfyV7SKfHZEPCKzGhn6pOP2)
(db에 dbwa 데이터베이스 만들기)

브라우저에서 dvwa github 검색(https://github.com/ethicalhack3r/DVWA/releases) -> zip파일 다운로드
압축 풀고 브라우저에서 접속 할 수 있게 폴더 이동(터미널에서 `mv DVWA-1.9 /opt/lampp/htdocs/dvwa` 입력)

브라우저에서 localhost/dvwa 이동.
(접속 안되면 터미널에서 sudo chmod -R 0777 /opt/lampp/htdocs/) 어차피 해줘야함
https://www.google.com/recaptcha/admin 접속
label에 **dvwa** 입력 -> domain에 'localhost' -> 생성
터미널에서 `cd /opt/lampp/htdocs/dvwa/` 실행 - > `gedit config/config.inc.php` 실행
reCAPTCHA 검색 -> public_key와 private_key 입력 -> 위에 db_password값 삭제(기본값)
**Create / Reset Database** 클릭

---

## Reference

[화이트해커가 되기 위한 8가지 웹 해킹 기술](https://www.udemy.com/everything-about-white-hat-hacker/learn/v4/overview)
