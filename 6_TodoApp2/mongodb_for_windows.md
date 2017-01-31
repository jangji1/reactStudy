# 윈도우즈에서 MongoDB 설치하기

## 개요

이 튜토리얼을 windows 환경에서 MongoDB Community Edition을 설치하는 데 활용하세요.

> Platform Support
> 2.2버전부터 MongoDB는 windowsXP를 지원하지 않습니다. 더 최근에 배포된 MongoDB를 이용하기 위해서는 보다 최신의 윈도우를 사용해주세요.

> Windows Server 2008 R2나 windows7을 사용하시는 분들은 [메모리 매핑 파일 이슈에 대한 핫픽스](http://support.microsoft.com/kb/2731284)를 설치해주세요.


## 요구사항

MongoDB Community Edition은 Windows Server 2008 R2, Windows Vista 혹은 그 이상의 windows 환경에서 동작합니다. `.msi` 설치파일에는 모든 다른 소프트웨어 의존성을 포함하고 있고, `.msi` 파일로 설치되었던 구버전을 자동으로 업그레이드 해줍니다.


## MongoDB Community Edition 설치하기

### 대화형 설치


파일탐색기에서 다운받은 MongoDB .msi파일을 더블클릭하세요. 대화형 프로세스에 따라 설치를 가이드해줄 화면이 나타날 것입니다.
"Custom" 설치 옵션을 선택하면 설치할 경로를 지정하실 수도 있습니다.

> NOTE
> 이 안내서는 당신이 MongoDB를 `C:\Program Files\MongoDB\Server\3.2\` 경로에 설치했다는 가정 하에 작성되었습니다. MongoDB는 독립적이며 다른 어떠한 시스템 의존성도 가지지 않습니다. 당신은 MongoDB를 당신이 원하는 폴더에 설치하고 실행할 수 있습니다(예. D:\test\mongodb).



### 직접 설치하기

당신은 `msiexec.exe`를 이용하여 커맨드라인 상에서 MongoDB Community를 직접 설치하실 수 있습니다.


#### 1. 관리자모드로 명령프롬프트를 여세요.

`Win`키를 누르고, `cmd.exe`를 입력하신 다음, `Ctrl + Shift + Enter`를 눌러 명령프롬프트를 관리자권한으로 실행하세요.

다음 스텝부터는 위의 관리자모드 명령프롬프트 내에서 실행하세요.

#### 2. MongoDB 설치

`.msi` 파일이 있는 경로로 이동한 다음, 다음을 실행하세요. (경로 `INSTALLLOCATION`는 원하시는 경로로 수정하세요.)

```bash
msiexec.exe /q /i mongodb-win32-x86_64-2008plus-ssl-3.4.1-signed.msi ^ INSTALLLOCATION="C:\Program Files\MongoDB\Server\3.4.1\" ^ ADDLOCAL="all"
```

기본적으로 이 방법은 MongoDB의 모든 바이너리들을 설치합니다. 특정 MongoDB 컴포넌트 셋을 설치하고 싶다면, `ADDLOCAL` 아규먼트에 다음 중 하나 이상의 컴포넌트 셋이 포함된, 콤마로 구분된 리스트를 특정하여야 기술해야 합니다.

|Component Set	| Binaries|
|---|---|
|Server|	mongod.exe|
|Router|	mongos.exe|
|Client|	mongo.exe|
|MonitoringTools|	mongostat.exe, mongotop.exe|
|ImportExportTools|	mongodump.exe, mongorestore.exe, mongoexport.exe, mongoimport.exe|
|MiscellaneousTools|	bsondump.exe, mongofiles.exe, mongooplog.exe, mongoperf.exe|

예를 들어 오직 MongoDB 유틸리티만을 설치하기 위해서는 다음처럼 하시면 됩니다.

```bash
Copymsiexec.exe /q /i mongodb-win32-x86_64-2008plus-ssl-3.4.1-signed.msi ^ INSTALLLOCATION="C:\Program Files\MongoDB\Server\3.4.1\" ^ ADDLOCAL="MonitoringTools,ImportExportTools,MiscellaneousTools"
```


## MongoDB Community Edition 실행

### 1. MongoDB 환경 세팅하기

MongoDB는 모든 데이터를 저장하기 위한 데이터 디렉토리를 필요로 합니다. MongoDB에서 기본으로 설정한 디렉토리는 MongoDB가 설치된 드라이브의 절대경로 `\data\db`입니다. 이 폴더를 만들기 위해 명령프롬프트에서 다음 내용을 실행해주세요.

```bash
md \data\db
md \data\log
```

> (역자주 : 개인적으로는 `C:\mongodb` 경로 내부에 db 및 log 폴더를 만들기를 추천합니다.)
```bash
cd c:\mongodb
md db
md log
```

혹은, 다른 폴더를 만들고 mongod.exe --dbpath 옵션을 이용해 이 폴더를 대체안으로 지정할 수도 있습니다. 예를 들어 다음은 `d:\test\mongodb\data`를 새로운 MongoDB 데이터 저장공간으로 지정한 것입니다.

```bash
"C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --dbpath d:\test\mongodb\data
```

만약 당신이 새로 지정할 경로 중에 공백이 포함된 경우에는, 전체 경로를 쌍따옴표로 감싸주세요. 예를 들면 다음처럼 하시면 됩니다.

```bash
"C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe" --dbpath "d:\test\mongo db data"
```

혹은 [설정파일](https://docs.mongodb.com/manual/reference/configuration-options/)에서 데이터베이스 저장 경로를 지정하실 수도 있습니다.



### 2. MongoDB 시작하기

MongoDB를 시작하기 위해서는 mongod.exe 파일을 실행해야 합니다. 예를 들어 명령프롬프트에서 다음을 실행하시면 됩니다.

```bash
C:\Program Files\MongoDB\Server\3.4\bin\mongod.exe
```

이렇게 하면 메인 MongoDB 데이터베이스 프로세스가 시작됩니다. mongod.exe 프로세스가 성공적으로 실행되면 연결대기 메시지가 콘솔 결과로 표시됩니다.

여러분의 시스템 보안레벨에 따라서 Windows가 MongoDB의 일부 기능을 차단하였다는 보안경고 알림창을 띄울 수도 있습니다. 모든 사용자분들께서는 반드시 '홈 네트워크' 또는 '직장네트워크'와 같은 사적인 네트워크를 이용하셔야 하며, 접근권한을 허용하게끔 버튼을 클릭하셔야 합니다. 관련된 추가정보는 [Security Documentation](https://docs.mongodb.com/manual/security/)을 참조하세요.


### 3. MongoDB에 연결하기

`mongo.exe` 쉘을 통해 MongoDB에 연결하기 위해서는 새로운 명령프롬프트를 열고 다음을 실행하세요.

```bash
C:\Program Files\MongoDB\Server\3.4\bin\mongo.exe
```

.NET을 이용한 어플리케이션을 개발하기를 원하는 경우에는 [C# and MongoDB](https://docs.mongodb.com/ecosystem/drivers/csharp/)를 참고하세요.


### 4. MongoDB 사용 시작하기

MongoDB 사용을 돕기 위해, MongoDB는 다양한 드라이버 버전에 대한 [Getting Started Guide](https://docs.mongodb.com/manual/#getting-started)를 제공하고 있습니다.

Production 환경에서 MongoDB를 배포하기 전에는 [Production Note](https://docs.mongodb.com/manual/administration/production-notes/)를 살펴보시길 권합니다.

MongoDB를 중지하기 위해서는 mongod가 실행중인 명령프롬프트에서 `Ctrl + C`를 누르세요.




## Trouble Shooting

- 경우에 따라 '한글폴더명' 혹은 '띄어쓰기'가 문제가 되는 수도 있습니다. MongoDB 설치 절대경로상에 한글이나 띄어쓰기가 있는 경우에는 설치경로를 바꿔서 다시 설치해보세요.
- 환경변수를 세팅할 때엔 mongod.exe 파일이 설치된 정확한 경로를 지정해야 합니다. 예를들어 mongodb를 `c:\program files\mongodb`에 설치했을 경우, `mongod.exe`파일의 경로는 `c:\program files\mongodb\bin` 입니다. 환경변수 세팅 관련해서는 다음을 참고하세요. http://dev.epiloum.net/537
