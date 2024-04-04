---
layout: post
title: "AWS EC2 Ubuntu 서버 초기 설정"
author: "Jiwon Min"
tags: [Post]
excerpt_separator: <!--more-->
---

`AWS EC2` 를 이용하여 개발 혹은 테스트 용도의 인스턴스 생성 후 가장 기본적으로 적용하면 좋을 Ubuntu 초기 설정에 대한 내용 정리입니다.<!--more-->

`Router 53`, `ELB` 및 `RDS` 등 다른 서비스와 보안 연결 설정 등은 다루지 않습니다. `AWS EC2` 라고 제목을 정했지만 엄밀히 말하자면, `Ubuntu 서버 초기 설정`이 더 맞을 듯 합니다.

<br />
<br />

### EC2 사양

- 소프트웨어 이미지(AMI) : ___Ubuntu Server 22.04 LTS (HVM), SSD Volume Type___
- 가상 서버 유형(인스턴스 유형) : ___t2.micro___
- 방화벽(보안 그룹) : ___launch-wizard-9___
- 스토리지(볼륨) : ___1개 - 30GB___
- 키 페어(로그인) : ___key pair___

![Create EC2 Instance](../assets/img/post/ubuntu-default-setting/1.%20Create%20EC2%20Instance.png "Create EC2 Instance")
_이 외의 사양은 건드리지 않은 Default 값을 사용합니다._

개발 혹은 테스트 용도이기 때문에 큰 사양은 불필요하고, `free tier` 사용이 가능한 모델로 서버를 구성합니다.

![Security Group - InBound](../assets/img/post/ubuntu-default-setting/2.%20Security%20Group%20-%20InBound.png "Security Group - InBound")

`방화벽(보안 그룹)` 부분의 ___launch-wizard-9___ 설정값은 새롭게 생성하는 경우 기본으로 적용되는 보안 그룹입니다.
`22`, `80`, `3306` 등 기본적으로 API 개발에 필요한 포트가 열려있고, IP 대역은 모두에게 열려있습니다.
__[OpenVPN](https://openvpn.net/ "OpenVPN")__{:target="_blank"} 같이 로컬 환경에서 IP 주소를 고정할 수 있는 환경을 만들고, 해당 IP 주소만 테스트에 필요한 포트에 접속이 가능하도록 설정하는 것이 보안상 더욱 좋습니다.

![Key Pair](../assets/img/post/ubuntu-default-setting/3.%20Key%20Pair.png "Key Pair")

기본적으로 ubuntu 계정으로 로그인을 하게 되고, 비밀번호가 아닌 키 파일을 이용해 접속합니다. 
__[PuTTY](https://www.putty.org/ "PuTTY")__{:target="_blank"} 를 사용하는 경우 `ppk` 파일을 받으면되고, __[Termius](https://termius.com/ "Termius")__{:target="_blank"} 같은 다른 프로그램을 사용한다면 `pem` 파일을 받으면 됩니다.

![Access Ubuntu](../assets/img/post/ubuntu-default-setting/4.%20Access%20Ubuntu.png "Access Ubuntu")
_Termius 접속 화면_

<br />
<br />

### hostname 변경
```shell
ubuntu@ip-172-0-0-0:~$ sudo hostnamectl set-hostname test
ubuntu@ip-172-0-0-0:~$ sudo reboot

# hostname 변경 완료
ubuntu@test:~$
```

<br />
<br />

### 우분투 업데이트
```shell
ubuntu@test:~$ sudo apt update
ubuntu@test:~$ sudo apt upgrade
ubuntu@test:~$ sudo apt dist-upgrade
ubuntu@test:~$ sudo apt autoremove
ubuntu@test:~$ sudo apt clean
ubuntu@test:~$ sudo apt autoclean
```

<br />
<br />

### history 형식 변경

![History Setup](../assets/img/post/ubuntu-default-setting/5-1.%20History%20Setup%20-%20default%20format.png "History Setup - default format")
_기존 history 형식_

```shell
# history 형식 변경
ubuntu@test:~$ sudo vi /etc/profile

# 가장 하단에 아래 내용 추가
# VIM 기준 Shift + G 누르는 경우 가장 하단으로 이동
HISTTIMEFORMAT="[%Y-%m-%d %H:%M:%S] "
export HISTTIMEFORMAT
```

![History Setup](../assets/img/post/ubuntu-default-setting/5-2.%20History%20Setup%20-%20Add%20HISTTIMEFORMAT.png "History Setup - Add HISTTIMEFORMAT")
_HISTTIMEFORMAT 내용 추가_

![History Setup](../assets/img/post/ubuntu-default-setting/5-4.%20%5BComplete%5D%20History%20Setup.png "[Complete] History Setup")
_history 형식 변경 완료_

<br />
<br />

### history cache 늘리기
```shell
# history cache 늘리기
ubuntu@test:~$ sudo vi /etc/bash.bashrc 

# 가장 하단에 아래 내용 추가
# VIM 기준 Shift + G 누르는 경우 가장 하단으로 이동
export HISTSIZE=10000
export HISEFILESIZE=10000
```

![History Setup](../assets/img/post/ubuntu-default-setting/5-3.%20History%20Setup%20-%20Add%20HISTSIZE.png "History Setup - Add HISTSIZE")
_HISTSIZE 및 HISEFILESIZE 내용 추가_

<br />
<br />

### locale 한국으로 설정
```shell
# 현재 locale 확인
ubuntu@test:~$ locale
```

![Locale Setup](../assets/img/post/ubuntu-default-setting/6-1.%20Locale%20Setup%20-%20Check%20locale.png "Locale Setup - Check locale")
_현재 locale 설정 값 확인_


```shell
# 한글 언어팩 설치
ubuntu@test:~$ sudo apt install language-pack-ko

# 한글 적용
ubuntu@test:~$ sudo update-locale LANG=ko_KR.UTF-8 LANGUAGE="ko_KR:ko:en_US:en" LC_MESSAGES=POSIX

# 재부팅 후 적용
ubuntu@test:~$ sudo reboot
```

![Locale Setup](../assets/img/post/ubuntu-default-setting/6-2.%20%5BComplete%5D%20Setup%20Locale.png "[Complete] Locale Setup")
_locale 변경 완료_

<br />
<br />

### timezone 한국으로 설정
```shell
# 타임존 확인
ubuntu@test:~$ timedatectl
```

![Timezone Setup](../assets/img/post/ubuntu-default-setting/7-1.%20Timezone%20Setup%20-%20Check%20timezone.png "Timezone Setup - Check timezone")
_현재 timezone 설정 값 확인_

```shell
#서울 타임존 있는지 확인
ubuntu@test:~$ timedatectl list-timezones | grep Seoul

#타임존 서울로 변경
ubuntu@test:~$ sudo timedatectl set-timezone Asia/Seoul 
```

![Timezone Setup](../assets/img/post/ubuntu-default-setting/7-2.%20%5BComplete%5D%20Timezone%20Setup.png "[Complete] Timezone Setup")
_timezone 변경 완료_

<br />
<br />

### 참고사항

- [How to set or change timezone](https://linuxize.com/post/how-to-set-or-change-timezone-on-ubuntu-20-04/ "How to set or change timezone")
- [How do I change the default locale](https://askubuntu.com/questions/89976/how-do-i-change-the-default-locale-in-ubuntu-server "How do I change the default locale")