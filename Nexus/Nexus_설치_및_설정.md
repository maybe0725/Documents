# Nexus 설치 및 설정

<br/>

### 1. 준비작업

- Java JDK 사전 설치
- 다운로드 : https://www.sonatype.com/download-oss-sonatype

<br/>

### 2. 계정생성

```sh
[CentOS]
useradd nexus
su nexus
```

```sh
[Ubuntu]
useradd nexus -m -s /bin/bash
```

<br/>

### 3. 설치 다운로드

```sh
wget https://download.sonatype.com/nexus/oss/nexus-latest-bundle.tar.gz
```

<br/>

### 4. 압축 해제

```sh
tar -zxvf nexus-latest-bundle.tar.gz
```

<br/>

### 5. Nexus 실행 설정 - nexus.properties

압출을 해제하면 `sonatype-work` 이란 폴더와 `nexus-[version]` 두 개의 폴더가 생성된다.

nexus-[version] 폴더(ex: nexus-2.14.15-01) 로 이동한다.

```sh
cd /app/nexus/nexus-2.14.15-01/conf
vi nexus.properties
```

```
# Jetty section
## WAS 의 app port. SELinux 를 쓸 경우 다른 포트로 변경한다. (Ex. 8080)
application-port=8081
application-host=0.0.0.0

nexus-webapp=${bundleBasedir}/nexus
## http://localhost:8081/nexus 로 연결해야 한다. /nexus 가 싫고 / 로 연결하고 싶을 경우 아래의 /nexus 를 / 로 변경한다.
nexus-webapp-context-path=/nexus


# Nexus section
## artifact 가 저장되는 폴더이다. 다른 경로에 있을 경우 수정한다. 이 폴더는 꼭 백업을 해줘야 한다.
nexus-work=${bundleBasedir}/../sonatype-work/nexus
runtime=${bundleBasedir}/nexus/WEB-INF
```

<br/>

### 6. Nexus 실행 설정 - 서비스 등록

Nexus 실행 설정은 `서비스 등록`, `환경변수 등록` 중 한가지 방법으로 설정 후 실행하면 된다.

```sh
cp /app/nexus/nexus-2.14.15-01/bin/nexus /etc/init.d/nexus
vi /etc/init.d/nexus
```

```sh
#NEXUS_HOME=".."
NEXUS_HOME="/app/nexus/nexus-2.14.15-01"

#RUN_AS_USER=
RUN_AS_USER=nexus

#PIDDIR="."
PIDDIR="/app/nexus"
```

```sh
### 서비스 시작
systemctl start nexus
### 서비스 종료
systemctl stop nexus
```

<br/>

### 7. Nexus 실행 설정 - 환경변수 등록

Nexus 실행 설정은 `서비스 등록`, `환경변수 등록` 중 한가지 방법으로 설정 후 실행하면 된다.

```sh
cd /home/nexus
vi .profile
```

```sh
NEXUS_HOME=/app/nexus/nexus-2.14.15-01
PATH=$PATH:$NEXUS_HOME/bin

export NEXUS_HOME
export PATH
```

```sh
### 서비스 시작
nexus start &
### 서비스 종료
nexus stop &
```

<br/>

### 8. 실행여부 확인

```sh
ps -ef | egrep "java|nexus" | grep -v grep

netstat -anp | grep 8081
```

<br/>

### 9. 접속

```
http://서버주소:8081/nexus
```

기본 `admin` 암호는 `admin123` 이다.

sonatype-work 에 artifact 가 저장되므로 이 폴더를 백업해 주어야 한다.

<br/>

### 10. maven lib 설정

- `Repositories` tab에서 > central 클릭 > configuration > `Download Remote Indexes` : `True` 설정 > Save
- Browse Index에서 cache된 내용 확인 > pom.xml 실행 > Browse Storage에서 다운된 lib 확인
- 진행중인 작업확인 > Administration > Scheduled Tasks 에서 작업을 클릭하여 진행상태 확인

<br/>

### 11. custom lib 등록

<br/>

### 추가 1. 사용하기 전 알아야하는 개념 - Repository Type

1. Hosted

```
기본 Type으로서 회사 내에서 개발한 jar 파일 또는 회사에서 제품개발을 하기 위해서 구입한 3rd party의
jar 파일을 관리하는 Repository가 이에 속합니다.

Nexus에서 기본적으로 제공하는 Hosted Type Repository는 Snapshots(사내 개발용 repository),
Releases(사내 제품 repository), 3rd party를 제공합니다.
```

2. Proxy

```
Global Repository처럼 외부 Repository에 대해서 proxy 역할을 합니다.

maven의 Central Repository는 매우 느리고, 최신 버전이 올라오는 데 굉장히 오래 걸립니다.

그래서 jboss, springsource 등에서 별도의 maven repository를 구축하여 운영하고 있습니다.

이런 경우에 각 개발자들이 해당 maven repository에 대한 설정을 각각 할 수 있기 때문에
Remote Repository를 외부 오픈소스에 대한 Proxy 서버로서의 역할을 수행할 수 있습니다.

Nexus에서 기본적으로 제공하는 Proxy Type Repository는 Google Code, java.net, Maven Central이 있습니다.
```

3. Virtual

```
서로 다른 타입의 Repository에 대해서 adapter 역할을 합니다.

현재 Nexus는 maven1 repository와 maven2 repository에 대한 atapter 역할만 제공하고 있습니다.
```

4. Group

```
여러 개의 Repository를 하나로 묶어주는 역할을 합니다.
```

<br/>

### 추가 2. 사용하기 전 알아야하는 개념 - Repository

1. Snapshots

```
빌드등 수시로 릴리즈 되는 바이너리를 배포 하는 장소
```

2. Releases

```
정식 릴리즈를 통해서 배포되는 바이너리를 저장하는 저장소
```

3. 3rd party

```
벤더등에서 배포하는 (Oracle,IBM등) 바이너리를 저장해놓는 장소로 특정 솔루션등을 사용할때,
딸려 오는 라이브러리등을 여기에 놓고 사용한다
```

4. Proxy Repository

```
원격에 원본 repository가 있는 경우, Local에 캐쉬 용도로 사용한다.
```

5. Virtual Repository

```
Repository Group은 몇 개의 repository를 하나의 repository로 묶어서 단일 접근 URL을 제공한다.
```

<br/>

### 추가 3. 개념정리

`Central` repository는 maven repository에서 라이브러리 목록을 index 캐싱 해오고, pom.xml을 통해 실행을 하는 경우에 proxy를이용해 라이브러리를 다운받아 놓는다

`third party` repository는 사용자가가 제작한 라이브러리나 벤더에서 제공하는 라이브러리를 저장할 수 있다.

`public` repository는 central 라이브러리와 3rd party 라이브러리를 통합하여 제공한다. 따라서 pom.xml에 public 라이브러리를 등록하면 모든 라이브러리를 접근하여 사용할 수 있게 된다.

<br/>
