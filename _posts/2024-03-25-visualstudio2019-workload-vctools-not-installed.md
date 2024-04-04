---
layout: post
title: "[solve] visualstudio2019-workload-vctools not installed"
author: "Jiwon Min"
tags: [Solve]
excerpt_separator: <!--more-->
---

> visualstudio2019-workload-vctools not installed. the package was not found with the source(s) listed.

Windows 11 에서 Node.js 설치 중 추가 패키지 설치 오류 관련해서 위와 같은 메시지를 받은 경우 해결 방법입니다.<!--more-->

<br />
<br />

### 시나리오

![Node.js Setup](../assets/img/solve/visualstudio2019-workload-vctools/1-1.%20Node.js%20Setup%20-%20Automatically%20install%20the%20necessary%20tools.png "Node.js Setup - Automatically install the necessary tools")

Node.js 패키지 중 일부 패키지는 C/C++, Python 을 이용하여 컴파일을 해야합니다.
그 컴파일을 도와주기 위해서 `chocolaty` 를 사용하는데, 기존에 설치 된 `chocolaty` 경로에 `visualstudio2019-workload-vctools` 를 설치하지 못하는 경우에 에러가 발생합니다.

![Node.js Setup](../assets/img/solve/visualstudio2019-workload-vctools/1-2.%20Node.js%20Setup%20-%20%20necessary%20tools%20for%20Node.js%20installing.png "Node.js Setup -  necessary tools for Node.js installing")

![Problem](../assets/img/solve/visualstudio2019-workload-vctools/2.%20%5BProblem%5D%20visualstudio2019-workload-vctools%20is%20not%20installed.png "visualstudio2019-workload-vctools is not installed")

Node.js 사용에 필요한 추가 도구를 설치하기 위한 CMD 화면이 표시되고, PowerShell 을 이용하여 설치가 진행됩니다. 
설치 중에 위와 같은 에러가 표시되고 아무리 다시 실행해봐도 클린 설치가 되지 않습니다.

<br />
<br />

### 해결 방법은 두 가지가 있습니다.

1. C:\ProgramData\chocolaty 삭제 후 Node.js 재설치
   ![Solve 1](../assets/img/solve/visualstudio2019-workload-vctools/3.%20%5BSolve%5D%20Delete%20chocolatey%20and%20reinstall%20Node.js.png "Delete chocolatey and reinstall Node.js")

2. visual studio 2019 - build tools for visual studio 2019 최신 버전 다운로드
   ![Solve 2](../assets/img/solve/visualstudio2019-workload-vctools/4-1.%20%5BSolve%5D%20Download%20Build%20Tools%20for%20Visual%20Studio%202019.png "Download Build Tools for Visual Studio 2019")
   _[설치 경로](https://my.visualstudio.com/Downloads?q=visual%20studio%202019 "설치 경로")_{:target="_blank"}
   ![Solve 2](../assets/img/solve/visualstudio2019-workload-vctools/4-2.%20%5BSolve%5D%20MSBuild%20Tools%20install.png "MSBuild Tools install")
   ![Solve 2](../assets/img/solve/visualstudio2019-workload-vctools/4-3.%20%5BSolve%5D%20Complete%20install%20MSBuild%20Tools.png "Complete install MSBuild Tools")
   _설치 완료_

   - 설치가 완료되면 업그레이드 실행 `choco upgrade visualstudio2019-workload-vctools -y`
      ![Solve 2](../assets/img/solve/visualstudio2019-workload-vctools/4-4.%20%5BSolve%5D%20Upgrade%20visualstudio2019-workload-vctools%20using%20choco.png "Upgrade visualstudio2019-workload-vctools using choco")
      _업그레이드 참고 [visualstudio2019-workload-vctools#upgrade](https://community.chocolatey.org/packages/visualstudio2019-workload-vctools#upgrade "업그레이드 참고")_{:target="_blank"}

<br />
<br />

### 설치 완료

![Complete](../assets/img/solve/visualstudio2019-workload-vctools/5-1.%20%5BComplete%5D%20The%20upgrade%20of%20visualstudio2019buildtools%20was%20successful.png "The upgrade of visualstudio2019buildtools was successful")
_설치 완료_

![Complete](../assets/img/solve/visualstudio2019-workload-vctools/5-2.%20%5BComplete%5D%20Successful%20installed%20program%20files.png "Successful installed program files")
_최종 설치 파일_

<br />
<br />

### 참고사항

- 실행환경 : Windows 11 (Version 22H2)
- 설치버전 : Node.js 18.17.0 LTS (includes npm 9.6.7)
- StackOverflow : [choco-install-visualstudio2017-workload-vctools-fails-error-the-install-of](https://stackoverflow.com/questions/65185384/choco-install-visualstudio2017-workload-vctools-fails-error-the-install-of "StackOverflow 참고사항"){:target="_blank"}