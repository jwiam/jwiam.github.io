---
layout: post
title: "[solve] Failed to build gem native extension"
author: "Jiwon Min"
tags: [Solve]
excerpt_separator: <!--more-->
---

> Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

`Jekyll` 을 이용한 `GitHub Pages` 구성 중에 기본 세팅 후 번들 설치를 하게 되는데, 이때 일부 Gem 패키지에서 표시되는 위 에러에 대한 해결 방법입니다.<!--more-->

<br />
<br />

### 시나리오

![Ruby Setup](../assets/img/solve/failed-to-build-gem/1.%20Ruby%20Setup%20-%20Set%20up%20MSYS2%20and%20development%20toolchain.png "Ruby Setup - Set up MSYS2 and development toolchain")

Ruby Gem 패키지 중 일부 패키지는 `C` 를 이용해 컴파일을 해야합니다.
그 컴파일을 도와주기 위해서 `MSYS2` 도구와 `Development Toolchain` 을 설치해야 하는 데, 루비 인스톨러를 이용한 루비 설치 중에 `MSYS2` 만 설치하고 `Development Toolchain` 설치는 건너뛰게 되는 경우가 있습니다.
이런 경우에 일부 Gem 패키지 설치 중 에러가 발생합니다.

<br />
<br />

![MSYS2 Setup](../assets/img/solve/failed-to-build-gem/2.%20MSYS2%20Setup%20-%20MSYS2%20set%20up%20using%20ruby%20installer.png "MSYS2 Setup - MSYS2 set up using ruby installer")
_RubyInstaller2 CMD : 1, 2, 3 입력_

<br />

![MSYS2 installing](../assets/img/solve/failed-to-build-gem/3.%20MSYS2%20installing.png "MSYS2 installing")
_인스톨러의 1. MSYS2 base installation 실행중_

<br />
<br />

`MSYS2 base installation` 단계를 완료하면 다시 루비 인스톨러 화면으로 돌아와서 `ENTER` 를 누르라는 것 처럼 표시됩니다.
그냥 닫게되면 설치 프로세스가 종료되고, 나머지 단계는 진행되지 않은 채 종료하게 됩니다.

<br />
<br />

![Problem](../assets/img/solve/failed-to-build-gem/4.%20%5BProblem%5D%20Failed%20to%20build%20gem%20native%20extension.png "Failed to build gem native extension")
_Ruby-FFI 설치 실패_

<br />
<br />

### 해결 방법은 두 가지가 있습니다.

1. 루비 인스톨러에서 `MSYS2` 설치 단계 모두 다시 실행
   ![Solve 1](../assets/img/solve/failed-to-build-gem/5.%20%5BSolve%5D%20MSYS2%20Setup%20-%20Choose%20all%20options.png "MSYS2 Setup - Choose all options")
   <br />
   <br />
2. 루비 인스톨러를 설치 할 때, `Ruby+Devkit` 버전으로 설치
   ![Solve 2](../assets/img/solve/failed-to-build-gem/6-1.%20%5BSolve%5D%20Install%20ruby+devkit%20RubyInstaller.png "Install ruby+devkit RubyInstaller")
   _[설치 경로](https://rubyinstaller.org/downloads/ "설치 경로"){:target="_blank"}_
   <br />
   <br />
   ![Solve 2](../assets/img/solve/failed-to-build-gem/6-2.%20%5BSolve%5D%20Ruby%20Setup%20-%20with%20MSYS2%20development%20toolchain.png "Ruby Setup - with MSYS2 development toolchain")
   _MSYS2 development toolchain 설치 옵션 선택_

<br />
<br />

![Complete](../assets/img/solve/failed-to-build-gem/7.%20%5BComplete%5D%20bundle%20exec%20jekyll%20serve.png "bundle exec jekyll serve")
_설치 완료 후 서버 실행_

<br />
<br />

### 참고사항

- Windows 11 (Version 22H2)
- Ruby 3.2.2 (includes gem 3.4.10)
- StackOverflow : [https://stackoverflow.com/questions/22544754/failed-to-build-gem-native-extension-installing-compass](https://stackoverflow.com/questions/22544754/failed-to-build-gem-native-extension-installing-compass "StackOverflow 참고사항"){:target="_blank"}