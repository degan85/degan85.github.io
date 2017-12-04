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
2. Kali Linux 설치 및 설정
3. XAMPP 설치
4. dvwa 설치

---

<a name="vb_install"></a>### 1. VirtualBox 설치

#### - 설치

![VirtualBox_install_1](https://lh3.googleusercontent.com/EIsNBtj_wvBNhPCRZEECl6yzuFI8PDbRfFb348d0c8As_sfX9XVtCfJPFCrjjyOu3IroPqiLYkjjFpKBlnTznNVbhuu2L3mCxocVtzDVQamosPKYTBV7WTMHVuYwqcokeiK5eDMM3BlC4sCrdXrB-dWjTopbbr1RY5RnsMqy4R3Qc4ye3afTRZJFzB86qvuSt38ortN0dryObOzFr--zTsb2u_aPK3VVqXtJ2Abz6IG-g-HvVWR2zT9xuC0A3nlHIhwU4EDrm-znsl8ijEbjinTmRfATSSkVo2l6gJTVztrL5muR13of_Km4tOWGjflvlxvgMtHG8TjMKSJeL8anvAXOGolVE_X3kJlqfZO6C9WJVueQAF9p5NOadT24eErw_WtjSaUiKFT2zBPnjnvzKBcRTvGV4Jc0sJ6SXDTxQbXEEqmHS8zwYuFlfT7SdqrMTFc7JO-cRP9cnVzeUcHhSKiMlAZG-ubIoFnGsfI01STV99u-zEbc0vtA1tg11DLGuBnVm-wsachuCdJN6D_XuEHe24tqRwj0-xm_rVe4BMhHv9qCaAPHnExCLz56aBPEEv_4vS7upJehnFQKZ6zlsdsaX81yUjnzGax1uHnXlA=w944-h423-no)
![VirtualBox_install_2](https://lh3.googleusercontent.com/PLzaIpmZKNw-vPVIYNvTds3RLR2GlqYk1B8Q2kKcDHPMyMmR-3ix0aZr3x7BZ0eWSZfX3FVtD2fa7xsUafHWWwl9YSnU2saltWAtGemaveDmKD7cDEhnJbe65XVwNFysNsd-oo05VSiQE2obA1HRkUAVDYx-ad1PvUdV-HgQZ8gb8Syjj8jhvDjs_0ovnIlvMvw1D2Kaobgi5Bt3O_d5YN-wGzBGT4kJVcRUzZM-zY54Hpulb2jQ6vZOPw7Ys2EdIhtoW0icnQ-OFU32XmxO4Dpipb3XnCT1nUr257S6OJ0QnhjzEF90ocATovZt4r6DgY2KQR1Pz_jxwPDbliiA96W_SWRsc8ecFqzGwMC_HkE20NfhetyixpFPrf7dWhaVu3fzyqZ0O-DaX40AmWaQbUsuHlqTtffKEgyY0mMF9ZQ1eZhXvOa87b0D0cUxO4472kyqgT7fHzfvDQFmNyZL1qvskDa0IA5XkQmahFDKPJwPSu1l9r4B9zp4VqTO2poE85j3_d_qJiNXwjQ0fJ2pLpiHRqx4PiYyf8PuIysd4QAIe2-QnapEUt6x_Y081mzAxG7yhXQ8eTVEGkvn5IwLW5WqIWNBSms3LnQxeP13IQ=w944-h467-no)
HOST 운영체제에 맞게 다운로드하고 usb 사용 등을 위해 All supported platforms도 같이 다운로드합니다.
둘 다 다운로드하고 설치를 진행합니다.

---
#### 2. Kali Linux 설치 및 설정

#### - 설치

![Kali_Linux_install_1](https://lh3.googleusercontent.com/ZmZk-8sFuqlM1w5iuE-w-8AhZuqH0teyKdvPkocizEd9XgYdlu_3zBsS3I5xzzxRtNN8O9dNMOATnTHXAjLOy1_gLi0pJCKDVePSgeWLkRMZsc_6xN4CZDJP9xBICpQDpMp-L7YHEVSQl3wVukkIOCNN8DDT6oAYGnJgF3ibAfghveAz0JBZORujJWIFSXCCcr9Op4kLP_xw2I7XyDmYAI8Qf9T5N5_hzPJl1PBcjh-uL5wM6MV0Uww1AwK5lT23eKyiAbBIWhPKTH2hUvemBHlOsioeb2WFRDMQpnyUayuYB-rCukF_kwxv9ElDoKfauSkGGJzag_sXnYcpG2J1pEhW4KVut1pbqtRZ9kyyHSk5D1esT5yn3kJeORRb8AaeKMnpjLj7zqCL35l_E4jHzBqNesoIM_EZRuyvrGJrVxjKd-9FsDVJBIho9zXTXC0bWdQm_mdVEW52CpLQfs_S1NZKoQNtsZa-QrSsKGdUZzuwPBsXOIe-koptjhc5rjyHU4tfFXgcCYFNuUTry4z0UaQpHLWef8jO7_mV0W9mFNocgkiXpgc0TRd5cGmh636JJTVkTZ5nVFv67M70oXm_uhOlbSv2-m5aaT0xQpFF8g=w944-h460-no)
![Kali_Linux_install_2](https://lh3.googleusercontent.com/KAnbOV17cd59Y9VxyoB3u9TBiZ80V6z0p5HBvy6FLzRvxWYw6RuKHwnW7a4TLYSKTbJJ4AbIqkBQct-KssDplvc9mr9fcKTAxCu4x0og6DBfirMY51--fUf3AqwSSTqvut1WDy9vRrWQX6hhdS6nu8iJJipP3OiPY-QQ1P4UpqMf4lcHpJS_tornir2eWGH_OGRLkSEF_2kydpR3okKC4xSrPtrF_a6CXrK2Mm5DHMwu-QEP0ot8jOcLwJsLV5GgNUs3gZtgL-SKi2ZVuYvNj2rscE9Xf-KVeFI04kzreiqiZCZn84TtFwdSxavZyO9EGW1HsXfYlEFgnVZlMTPFIEc0f2Dc1TFG6A19mOad93iVDqwgQdrHUlO5smHq22jBsn468jQiisavt28U4rRtNNy0cDbwsJSqViUcgV1428vhpw_Xi8qh7DF0lyIMXBvhoWG32z6_vmmb4XtslPUUAC8zFwnK3E3BXt2VgREVpdtnoG2VAzzJZJNIw6QSHYZSKK1ZY7sqPST_nlfhSFfTE3Unwohx18Bc561VuuOn0V_GcuhCLe53bS8UQLz8o_u4BBFPEcjjZxmjVsChX8-d3X19XrdNGRz-J_bF96LBKA=w944-h459-no)
![Kali_Linux_install_3](https://lh3.googleusercontent.com/t9Cs2mXFr9tDZvcu2iuODwsYZXJsKZ43ui7e0N9GBRqzViWZ0KN2ltJd5JXuzG-a81hSCpCsgNdD9skvoBeEoCX_U4pEVKKVIb-NMUNB_acdWod1aLjyvFyBJYWO4ZcbAF-2X6QC2tfajYjz5259dJbYcDyzZ3dHHa0cwwgr71_ZttwiIUaiXP2MjnxNUBgVvmYzO69GVpt8q7lhDT9rX4Fe4iqgQoZt4r1lSHWIHCGdJ1S_62XtQ89HaWih3COf2ex-c5ttosgYhxwgVy0whSY66nxsiNAGOA1zHIWaiW_sobyH1tW1rNfAU_XzGxGLkjaN-Gv8ZiGDzS2lrkaKu6SQB_bclLI-M_aKidMW2gr4NR3guvO3d7oYxzHCwl0vR9HWOAtJscLjI9ZmvL5mKabUFXbS-l4kRKY1EHjqI-OUilmqp4gDyP1dSHDVJmZnrjP9qZn5Mw5tLYhHKGXz0cWFsWtOZb9SIa3hJPA9o1cMxnJG-en6SGQIbDuR1kzSWmE_2qN4j4nEONRtNTBSthm4HkdbpNsicyhdcOcZT9W9JFBLeDMseOjKAUAZiwFY53RvhGpebSIcVxTO3bO4k0t0RoJTtcVuYlO1UhIprg=w944-h478-no)

**VirtualBox Images**로 다운로드합니다.
실행하면 VirtualBox가 열리고 가져오기를 합니다.

![Kali_Linux_install_4](https://lh3.googleusercontent.com/DDhsBHceaJAhl7gYjUAiK6__mBEE5Wt-XD-2VcLMW75E4fKU4P_KWohSFZ1eNLQytf_1ua82vEC2E8AWCUJwkjw7dRJ-j2DXbs8WN_K4o6KBxDR4W2RE_FVxoV9H2L2HyPQHr64qjuIybh2SCF6KesgmBgTdIQuxPSDb-8eLZVR_clPVpXJIPIng6x1CZR50Fwzb_7oh48eFGfwEdRCizqi2WTsN03kgX8TcCVUClQNz4q75-j5AfsTp0Roja9r6dWG_4YaiMS5V0TZ412O5iGlnbDYtzZMuU3-u5eO4TuTrPaD-mYkjgICpppk7A4cFSRhvYcM69Vk61Y1UTguaEI_4f6bifGcIzaYjSaUgPPBL6XcGr4uYyLUBzfQa-3mzbpzkBhAvHMKXvcL-VTR7w47ORRFS7zu6znZEbhy4bIx1kxJOmz0iFKuvAOgo89nMvCF7TIctTGkuZYej41zJKnP4l1tHv6sdiJ4PBjj8Yyo7-gAvm4AI8_0bCg09aKVXi1FWFLDqWAwXri6mOzfsVZ6fyiybBRrZd2V1cdSfS9DpughNS84XzUOa9eRTq2x1OoCE8OGyWr3Kl--xEtC22T2OIrkQNbSljN2XBjH8qw=w944-h517-no)
마우스 오른쪽을 누르면 시작, 정지, 설정 등을 할 수 있습니다.

![Kali_Linux_install_5](https://lh3.googleusercontent.com/VEEocDc5G4QvkgLzgw6Kv7gDYiE_sDGtpknfBZgdulDz7yECsXz4SjLA_iPqc16OHW-kbNqSjsgufnCs22-6L3-sUDp98ofJmLYR-09IqjSt-z9-Cyn8R-Fy0zIPojufTuZeWt3ow9XndiBtpVM3FTFCiLQNGX0Gm7XzcRyFYbzgA3YPt-4l5teuvm1ixg7MdOgwNBplIwW8SzvHqPnwK75XgR2JgIXPriGa_DOES9maXxQoqm_sMbSXq4oX6kcEMDJ6KBd5_Sz9pCn3DWlVldsCtzGxJdPcDDodYMaWnvnHkOc04wyfX7lwoJJO9f3RtQR7QTr9TIKHXJGxTECtp8IxHz2Js-d6vKChHk-7fi7zz2R3FClx0SD4XnDG82wQF6vPGAbHItbu-lzIMy-y36_VGcQkhY3si-_zj38dN9vsKRGjs8CcLrLTJeZpRv5kH7bs22dlPgpbDPMLyjNZ2WrwXXkbC8mqrFkO4baPWgznHn9Vp-wQy-r3c9Q_obJx2V9zBgHQ9h2HxGnwhR4w34SPComGC-fdVvqJugjFWUlPvC4s50-InZsOxheCFkhnC9cCaHPMDxkQlA3_iQK7DRnt1KAxLTvWq0zTS_ZhpA=w944-h515-no)
설정을 누른 후 **시스템**을 누릅니다. 기본 메모리를 여유롭게 잡습니다.(2GB정도)

시작하면 사용자 ID와 비밀번호를 입력해야합니다.
초기 사용자 ID : root
초기 비밀번호 : toor

#### - 한글 폰트 설치
터미널을 열고 `apt-get install -y fonts-nanum` 입력합니다.
완료되면 `fc-cache -f -v`를 입력합니다.

**settings**에서 **Resion & Language** -> 맨 아래 **한국어** 설정 -> 다시 시작

#### - 한영 변환
터미널에서 
`apt-get install -y nabi` 실행합니다. 
`apt-get install -y im-config`을 실행합니다. 
`im-config` 실행 -> 확인 -> yes -> **hungul** 선택 -> 확인 -> 로그아웃
`shift` + `space`로 한영 변환됩니다. 

![Kali_Linux_install_4](https://lh3.googleusercontent.com/Yq5JIFAsgiBwp2Na7yr1N5ZwMmXyfPe0c6skRcmZroyk27djauy882KiGU30i44AYWrefAu0rTWWj4dd-lHWpNxWajfe4Aicnfnuj03ZQXFhAKL1uQgul1kOU2hnTRvDHSAcF3cYCEGg05Pey8uJaGtAPEv3uCfqTIOie1yz9oCXJMT-nJxBdLn89ilu64dm2RifM6CkeHGkdiCN_JbzWkpmTOC7ODgITkR1Yub4qxVcxZBqnwx4ZtOFRVPS6eAQtCD06feqyL9EDh9VGu-cNTkdV0BLspwQikOYJlIqmDqNU0NZiuTrmX0wBNuHGPQxeAsVD-I8bR36Mid05jsqmwE5TwqLY19GGGCfG0YZTO1VETKCCyCsVh8_8iTBSDYfo2rvbxya5vnkZBPg-W4ZNETVxrEaDO9U3IZYMJB8TJLs_mdHtrM0mWb1TtSxtQysfKFZpi_gVk0lFq6YuzA4tYe9vwgjkWU3WTXJ20L_8dSspDPuNko5xm8U_sPnM9ZAqfVBLrXIUBYMxTRariacJOeAscmp_lQcwRK4MTDEYY7UOi857XzGiQmXfQwXcJVQbDEpkW3bREpmME5EDcFCZACtFIfiQp-Ro_STlx6TbA=w944-h733-no)

#### - 스냅샷 복원지점
![Kali_Linux_install_5](https://lh3.googleusercontent.com/GA93kO4p-unHk1La9N_L08mTMc7W5DFv1vhEtoTrnwqYWgqA5D5DEY13-1bT_0LCjAQjxjGw5dp4rTFDvdrrA54iD5dTPyrsQaWJlzVed4P4SeldwnELTZb0xCA2KmbPyoAnX7xj7COqg-D-CYkM9aVzQB1DmWsHEpeOk6SoYlym_nSdejR0TvTAMxGrYeZTn0XKLibCg9hz3QywWdbFwM8n1BDDjj0e8hAV6YUn9JBgxh4W8AAlx7c8HIVH7s6yRfEIoIP_af-rGqorKjJhYWcWNqgl2tyLlYd6A2APVKkeiQW0kh_BLDbq0LAJ4vvp620ewgSCzxnQytQJh-HGGiXA0GYNos5H8x8swmp-DWd23kx9XJPJ5_Fae2scP3MkCiiVQWL1EEOdjwjQ8pyeF2Xls0OMCqH0YgOxqzBUhPtlX-KfOllghROZZjergH07cz93_o_HzbKnk7FqDs0A-j0OlO1xAX5-xzgX50NFl0BPK2P4Xhk_SMkNtL-AiZ5nIBPQ0oYp1K_gYrLoEMSN21-URdOiKY0nI902H7z0-DSKhWslGpZqsD-bMqb9xhPPoDKxdlM6Rm0qTE2B71VXGUY8dg31swwIRI8Rh5-I5g=w944-h510-no)
(스냅샷 -> 찍기) 
스냅샷을 찍어 복원지점을 만듭니다. 

![Kali_Linux_install_6](https://lh3.googleusercontent.com/ypig3x_76wYgU8nGCRkQK9i6xKJ1kwLAHRcekiCds7fmyUf0TkU19KaoI5C6_8a_iimcqEV0wkYSFhNWn6ZyQXSqPcZEQJJ4frssxdUhtuT1GfM-7jgXEKk_pfgp3G52NiJatp36lunc7AhEyJEudcf8M0cAHqCLWXCZkCarWMwUJEAGwXxoP2ZgKJnh22eWoA0t4SBdsYM28sxrjskgVrSg-DlSWHYKbG_cBGQa_1u8dr-aOd-yxUEvLwhZWKRhFSNvc0ZlAgxw-ig85Qnfy9kdwWnz80PbY9mlHF9c4CXSyalDSnxD0IvotiDa9dMAGtWaQoEzK5Md-yLrgvksQeoD7jGQqwdka2symDBLMPUmqsXu9Hty3P5s_SM7sC_FkzrJuwrOJ26LfnYnIrcx_tJoTXSv0BVeVUiMaiM5fy8tUkdAriCr_3xrXdDj4YdoV28wxqectD4aqgxZ-ruk-2iDLfx-7CWgev4uh1vZlJqt1RYJM6Lw6rouzgPGZVsXNeK6PzzGMakxEoNdUgFqAWmudvRSNHbkYCsuIS0rwRQmh07PVeZHCn6Ts8jM9Or2pxbJDuz9N2lcu54VuixHGU71AyBAhEanp_ubaJka1Q=w944-h510-no)
(복원 시 마우스 오른쪽 '복원' 선택)
문제가 발생 할 경우 복원지점으로 복원할 수 있습니다.

---

### 3. XAMPP 설치

#### - 설치
![XAMPP_install_1](https://lh3.googleusercontent.com/LCrhW36slCVqXCm7j0ph65wQPk4Vrb8vyqhByQhXXl2K5Z6TuwW6uU3JtsvhYrt5u80P3rR47FnEsNBy_WMH547AOI0j7snqTDDrCEMgeRfjEQ6Cq25O-XYvFnG0OeamCTXw6z3QiEzxIq8A9Nf7v1T0ITVT1WWRs1xJlFBoQmM3dHIkC6QzLFGoBAzYA9e3FsYaX7EfYOZzUxH_qz6qxpwrwFFRfyG8cZ5N6xTOHvDYRgjlrhkXpAJ--TQk_TrGDt3KT7thwOGoA1HUgRBPsDdPfWfV3Y-0dF-9XwuANBzq_oU6xRpzOEZLBIn_Phuo9rBAWaM9HA79CLOKNnFxXzbXYs8K7QbYDusiOW8xW0tXQSwopLYMncLmxEr1Dn_NNeFMTvwpAjhs5kCHX3mKeYYSin4lLZQkkafQw9NfrDvVXheGE2Xzr6-jpv_amI1yjb1p-Dzfe-aWAQD7E4hvH_tM30ApJa-c4Phsj-2AQHZgB_hA8SnGtYckTB7IUGssmvn8R396271-FiL98ImYvP9vLnskocFMMHszcneaMDQd5LO8_E0NjjPRNP9cAM5BB-ql2WRECZPTP1kotWxQrres1u7n3RUCCj0XIS8TSA=w944-h659-no)
![XAMPP_install_2](https://lh3.googleusercontent.com/J_VKwNJg5WmfgM4of6kGA1BiTpQT3w2X5aYs5Mnit7qyu0ggJ4JaBVlG-X-p80Y8EObA-__4J7QWY-kIkz0p8mfdviU6u5SxqkQDx8_eb-CFwabwMwKQd6j5c3M_2eM4N-BDltu45MdtG9WmITtRziTU7l2AQ9PFjQmJuRTaQyRKlJQ-Cddu3ftbll7oTsZqSlv4qHRtSYUtfAJvoUCcGeuzLrfR0zOHA9dz34zB6kre1a0qIP56oW5QQZTHv_GCtTwEYjw6pcUXQUK_qLu0MWo4l3yjglfuo-0DhdocZCexhqA6czo19yxh9_6ViEs0d-fXRf25tVIINYipOj2KploNu8wzGxtipOC_XHd32nrv-Mn5TxmgEOuvPYjhuN1ghEPW3cB6ojExd32WSN_854XnEtL-3Oq-QDoU3_8gjYEUzVacP4lij1fZiobchQDPGFRcz_iBapUBBGhKqkdpM308i4Mc3Yxn3H3noGjtOKEXfndq0QPo-lSewo_D5XGWYueMIQmQN-R9xLWcUFIuZeB8OhbLoeXzdpS_JtCunDVLOkEB9Oe6vY04I4OF26-bMW30grAVHfS56jVitxVcJl09F7pmNooMFKheDuwqBw=w944-h500-no)
(DVWA 지원을 위해 **5**점대 사용)

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
![DVWA_install_1](https://lh3.googleusercontent.com/W3ZtkucbN_acvamo87OVKY-ZmJyQTuvbdraIs2tubE186LaF9SdQPdG5E_pIxbNYxGxbjgbmOYaElFjpWb0w1AQ0pc-nxDRLCRjkJetYl7rc1V35agG32CXnnmnfw5uxuqGN3YOJdLHIrb1ZKFG_jDlEquOHjwC7gJZK9LlKaIZQ37heqAw6KguqI8L2TKAEiQNonNESUMj7T6WwNYG31rHMtj0WVbELuLk2f_2763ppOm-SL7QwRpG124uFOAcX9A4yleHm1oJdye8xJaIrdtJKvia6TvDEFPqrjsUoEX95iLNtWuvXluC8pI4dkKrmgpaXP361931txx4XPFkla6V9d4z0N2QmahKdS1Dbmozifp7JyTpPUB6JuSieDL0x5rdIOzEnU8OqiHiukW7HYHFBbH1hxGUH4zKxXX8f1_L7XHB8C2GJIJf3vR5d0Y7_7ExaS85KnHRL-c3VMJHayigsgFqy-UWiUPUW_3n7RXS25pWaAEgTqRhXhsJZzdqJL2Gxy35aRW4jJEapshkP5kkmx0IaGMoJeI4b7olzxZBE3_pHfn3xl5sivf-Q8LnBx6daNLabk7iQylggyd8qi4hI0nHmp_hmUp8TxPRiIg=w875-h948-no)
(phpMyAdmin 클릭)

![DVWA_install_2](https://lh3.googleusercontent.com/edyLJa8tFJesmvXt8ipGExLYaNP17VieOjx3t8GY_2LNyr03YoQp_6LOlzdgHKPJIg1HhCA1pA80XdqyDXUOwaPbbVDO87mwzT3w2IckHHZvZdxbW2W-P6LNBQYuCSl19T7qQzJdvVSHbzEXAXz5DUbaFaajF6M8rQUjPHnREjrmtXIQBqI9b3_ez60JJfFlrtNQdhrtb9K_2vgHlPuHbM6NeqZ04D0n5tZQDCVhZCwZwpvEf4ShAuvV1pmS0o66KhmB1EG5BILD_Lx32t2TJR3jeBO2rIMwx2Tz3LAUqe4-0q68irx2_4-bcfPPjTp6M9ifwNotcdbaZ1wQMKHxv1VLRj6iNZcKUmwkkFABLqfiX26ldSlljWFRv35X-X6_ijeQgEioohavHiSgXJVHJhCptNa92bl6NSyIENhV_kN5o_XgbrtelLs6mzCydAOpcbGzDbQWVl8VvXJ3aTYxKAf-b7a0u3wSJw4lYvYMi6PcIPR_lfSE6bki5IkfmVPACbeemoTkJHAOoSkVdq1PT_jy3X0DEw5JoD7d51lR60_7vke7vEq5qWWzo3h5tGKYCY-_c62l0xXNAAW3Cxl_qt2skcWyZB1s-2qHVQa_og=w944-h927-no)
(db에 dvwa 데이터베이스 만들기)

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
