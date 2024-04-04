---
layout: post
title: "[solve] DBMS: MySQL (no ver.) Case sensitivity: plain=mixed, delimited=exact"
author: "Jiwon Min"
tags: [Solve]
excerpt_separator: <!--more-->
---

> DBMS: MySQL (no ver.)
Case sensitivity: plain=mixed, delimited=exact
NotAfter: Wed Jun 01 12:00:00 UTC 2022.

`Amazon RDS` 데이터베이스를 `DataGrip` 에 연결할 때 정상적으로 연결되지 않고, `DBMS: MySQL (no ver.)` 이라는 메시지와 함께 실패되는 경우 해결 방법입니다.<!--more-->

<br />
<br />

### 시나리오

![Problem](../assets/img/solve/datagrip-mysql-error/1-1.%20%5BProblem%5D%20DBMS%20MySQL%20(no%20ver.)%20Case%20sensitivity%20plain=mixed,%20delimited=exact.png "DBMS MySQL (no ver.) Case sensitivity plain=mixed, delimited=exact")

MySQL 드라이버를 정상적으로 연결하지 못해 지속적으로 연결 실패가 표시됩니다.
자세한 에러 메시지를 확인하기 위해서는 DataGrip 로그를 확인해야 합니다.
```shell
# DataGrip 로그 파일
vi ~/AppData/Local/JetBrains/DataGrip{{ VERSION }}/log/idea.log
```

![Problem](../assets/img/solve/datagrip-mysql-error/1-2.%20%5BProblem%5D%20SSLHandshakeException%20error.png "SSLHandshakeException error")
_SSLHandshakeException error_

<br />

![Problem](../assets/img/solve/datagrip-mysql-error/1-3.%20%5BProblem%5D%20Communications%20link%20failure.png "Communications link failure")
_Communications link failure_

로그 파일 내에서 `Connecting to: jdbc:mysql://{HOST}:3306` 으로 검색하거나 연결을 시도한 Host 주소를 검색해서 어떤 에러가 발생했는 지 확인할 수 있습니다.
`javax.net.ssl.SSLHandshakeException` 또는 `[08S01] Communications link failure` 같은 통신 오류가 확인되었습니다. 

`TLS` 통신 과정에서 문제가 있는 것으로 보이고, 확실하지는 않으나 `Amazon RDS` 인스턴스 설정 중에 `스토리지 암호화` 값이 `활성화` 되어 있는 경우에 문제가 생기는 것으로 보입니다.

<br />
<br />

### 해결 방법은 두 가지가 있습니다.

1. `Driver` 를 `Amazon Aurora MySQL` 으로 변경
   ![Solve 1](../assets/img/solve/datagrip-mysql-error/2-1.%20%5BSolve%201%5D%20Change%20driver%20to%20Amazon%20Aurora%20MySQL.png "Change driver to Amazon Aurora MySQL")
   _Amazon Aurora MySQL 으로 변경_
   <br />
   ![Solve 1](../assets/img/solve/datagrip-mysql-error/2-2.%20%5BSolve%201%5D%20Better%20driver%20Ignore.png "Better driver Ignore")
   _알림 팝업에서 Ignore(무시) 클릭_
   <br />
   <br />
2. `Advanced` 에서 `useSSL` 값을 `FALSE` 으로 변경
   ![Solve 2](../assets/img/solve/datagrip-mysql-error/3.%20%5BSolve%202%5D%20useSSL%20FALSE.png "useSSL FALSE")
   _useSSL 값을 FALSE 으로 변경_

<br />
<br />

보안 연결이 해제되어 있는 부분은 그렇게 추천을 할 수는 없겠습니다. 그렇기 때문에 `Amazon Aurora MySQL` 또는 `MariaDB` 드라이버를 받아서 사용하는 것을 추천합니다. 

![Complete](../assets/img/solve/datagrip-mysql-error/4.%20%5BComplete%5D%20DBMS%20connection.png "DBMS connection")
_DB 연결 성공_

<br />
<br />

### 참고사항

- Amazon RDS - MySQL Community 5.7.38
- DataGrip Log : [https://intellij-support.jetbrains.com/hc/en-us/community/posts/10252570443282-Datagrip-cannot-access-containerized-database](https://stackoverflow.com/questions/22544754/failed-to-build-gem-native-extension-installing-compass "Datagrip-cannot-access-containerized-database"){:target="_blank"}
- DataGrip DB Connection Error : [https://youtrack.jetbrains.com/issue/DBE-13313/Cant-connect-to-remote-MySQL-since-last-version-of-IntelliJ#focus=Comments-27-4921748.0-0](https://youtrack.jetbrains.com/issue/DBE-13313/Cant-connect-to-remote-MySQL-since-last-version-of-IntelliJ#focus=Comments-27-4921748.0-0 "DataGrip DB Connection Error"){:target="_blank"}