# Ubuntu 18.04 OpenJDK 수동설치 및 update-alternatives 설정

<br/>

참고글 : [LINE의 OpenJDK 적용기: 호환성 확인부터 주의 사항까지](https://engineering.linecorp.com/ko/blog/line-open-jdk/)

<br/>

![images](../../../Images/20191024/2019-10-24_17-48_01.png)

<br/>

### OpenJDK 배포 버전의 종류와 차이

| COMMUNITY<br/>VENDOR                                         | PRODUCT NAME | OSS<br/>COMMERCIAL | ARCHITECTURE       | DESCRIPTION                                                                                                                                                                                                                                                                                                                                           |
| ------------------------------------------------------------ | ------------ | ------------------ | ------------------ | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [ORACLE](https://www.oracle.com/technetwork/java/index.html) | Oracle JDK   | Commercial         | Hotspot            | · Java 8 2019년 1월 이후 License 필요<br/>· 검토 시 기준 값으로 사용                                                                                                                                                                                                                                                                                  |
| [AZUL](https://www.azul.com/)                                | Zing         | Commercial         | Zing               | · Azul의 자체 아키텍처를 가진 JVM입니다.<br/>· 원래 상용 버전아키텍처 차이 및 상용 제품으로 검토에서 제외                                                                                                                                                                                                                                             |
| [OpenJDK.org](https://openjdk.java.net/)                     | OpenJDK      | OSS                | Hotspot            | · 썬 마이크로시스템즈 시절에 만들어진 커뮤니티 그룹으로 우리가 알고 있는 OpenJDK가 만들어지는 곳입니다.<br/>· 커뮤니티에 집중되어 있고 Source는 배포하지만 Binary 배포는 https://jdk.java.net을 통하여 이루어 집니다.<br/>· https://jdk.java.net에서 배포되는 Binary는 참조용으로 사용할 것을 권장합니다. [(참고)](https://jdk.java.net/java-se-ri/8) |
| [AZUL](https://www.azul.com/)                                | Zulu         | OSS                | Hotspot            | · Azul에서 오픈소스로 제공하는 JDK입니다. Zing과는 아키텍처 자체가 다릅니다.<br/>· OpenJDK를 기반으로 개발됩니다. 별도의 계약으로 지원을 받을 수 있습니다.                                                                                                                                                                                            |
| [Red Hat/CentOS](https://access.redhat.com/articles/1299013) | OpenJDK      | OSS                | Hotspot            | · 레드햇에서 제공됩니다.<br/>· RHEL(Red Hat Enterprise Linux) 또는 CentOS yum repository를 통하여 rpm(binary)으로 배포됩니다.<br/>· OS 버전에 따라서 지원되는 버전이 조금씩 다릅니다.<br/>· [개발자 사이트](https://developers.redhat.com/products/openjdk/overview/) (Windows binary)                                                                |
| [AdoptOpenJDK.net](https://adoptopenjdk.net/)                | OpenJDK      | OSS                | HotspotOpenJ9(IBM) | · OpenJDK 배포를 주 목적으로 하며, 여러 기업의 후원을 받아 개발자들이 운영하는 커뮤니티입니다.<br/>· 오라클의 지원으로 OpenJDK.org가 주도하는 Hotspot(openjdk)과, IBM의 지원으로 Eclipse가 주도하는 OpenJ9 VM의 Binary를 모두 제공합니다.<br/>· 현 상황에서 가장 다양한 플랫폼과 VM아키텍처의 Binary를 제공 받을 수 있습니다. (PC용 MAC OS 포함)      |
| [Eclipse.org](https://www.eclipse.org/openj9/)               | OpenJ9       | OSS                | OpenJ9(IBM)        | · IBM이 자체 JDK를 Eclipse에 기부하면서 시작된 프로젝트로 VM 아키텍처가 Hotspot과 다릅니다.<br/>· 기존에 IBM 계열의 JDK를 사용 중이던 기업에서 OpenJDK를 고려할 경우 OpenJ9을 고려할 수 있습니다.<br/>· 기존의 Hostpot 계열의 JVM과 설정 및 아키텍처에서 다수의 차이가 있습니다.<br/>· 아키텍처 차이로 인하여 검토에서 제외                           |

<br/>

### Download Link

[AdoptOpenJDK]

- https://adoptopenjdk.net/
- https://adoptopenjdk.net/releases.html

[AdoptOpenJDK - OpenJDK 8 (LTS)]

- https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u232-b09/OpenJDK8U-jdk_x64_linux_hotspot_8u232b09.tar.gz
- https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u232-b09/OpenJDK8U-jre_x64_linux_hotspot_8u232b09.tar.gz

[AdoptOpenJDK - OpenJDK 11 (LTS)]

- https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.5%2B10/OpenJDK11U-jdk_x64_linux_hotspot_11.0.5_10.tar.gz
- https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.5%2B10/OpenJDK11U-jre_x64_linux_hotspot_11.0.5_10.tar.gz

<br/>

[OpenJDK]

- https://jdk.java.net/
- https://jdk.java.net/archive/

<br/>

[AZUL]

- https://www.azul.com/
- https://www.azul.com/downloads/zulu-community/

[AZUL - zulu JDK 7 (LTS)]

- https://cdn.azul.com/zulu/bin/zulu7.34.0.5-ca-jdk7.0.242-linux_x64.tar.gz

[AZUL - zulu JDK 8 (LTS)]

- https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jdk8.0.232-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jre8.0.232-linux_x64.tar.gz

[AZUL - Java 11 (LTS)]

- https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jre11.0.5-linux_x64.tar.gz

<br/>

### 자바 홈 폴더 JAVA HOME

`update-alternatives`와 `alternatives`는 여러 버전의 패키지를 관리할 수 있는 Linux에서 제공되는 도구입니다.

여기서는 `Ubuntu`에서 쓰는 `update-alternatives` 를 기준으로 설명하겠습니다.

`apt` 로 설치한 JDK는 `/usr/bin/java` 에서 심볼릭 링크로 연결됩니다.

이 심블릭 링크는 `/etc/alternatives/java` 를 중간에 거쳐서 실제 설치한 디렉토리로 연결된 다는 것을 아래와 같이 확인할 수 있습니다.

```sh
➜  ~ ll /usr/bin/java
lrwxrwxrwx 1 root root 22  6월  9 22:20 /usr/bin/java -> /etc/alternatives/java
➜  ~ ll /etc/alternatives/java
lrwxrwxrwx 1 root root 43  6월  9 22:20 /etc/alternatives/java -> /usr/lib/jvm/java-12-openjdk-amd64/bin/java
```

`readlink -f /usr/bin/java` 명령으로도 동일한 결과를 볼 수 있습니다.

이 링크는 `update-alternatives` 로 관리됩니다.

아래와 같은 명령으로 현재 설치된 버전들과 우선 순위를 확인할 수 있습니다.

```
sudo update-alternatives --display java
```

수동으로 다운로드 압축을 풀어서 설치하거나 SDKMAN, Jabba등으로 설치한 JDK가 있다면 아래 명령으로 `update-alternatives` 의 관리대상에 추가할 수 있습니다.

```
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_31/bin/java 1000
```

심볼릭 링크로 연결되는 버전을 바꾸고 싶다면 아래와 같이 입력합니다.

```
sudo update-alternatives --config java
```

설치된 버전을 확인하고 번호를 선택해서 심볼릭 링크를 바꿀 수 있습니다.

```sh
There are 4 choices for the alternative java (providing /usr/bin/java).

  Selection    Path                                            Priority   Status
------------------------------------------------------------
* 0            /usr/lib/jvm/java-12-openjdk-amd64/bin/java      1211      auto mode
  1            /usr/lib/jvm/java-11-openjdk-amd64/bin/java      1111      manual mode
  2            /usr/lib/jvm/java-12-openjdk-amd64/bin/java      1211      manual mode
  3            /usr/lib/jvm/java-13-openjdk-amd64/bin/java      1211      manual mode
  4            /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java   1081      manual mode

Press <enter> to keep the current choice[*], or type selection number:
```

그런데 명령행에서 실행한 `java` 가 어느 곳으로 연결될지는 환경변수 `PATH` 에 영향을 받습니다.

`/usr/bin/java` 보다 더 우선 순위가 높게 먼저 선언된 디렉토리에 `java가 있다면 update-alternatives` 에서 지정한 java가 실행되지 않을 수도 있습니다.

SDKMAN, Jabba 등을 함께 사용한다면 이 점을 유의해야 합니다.

현재 쉘, 디렉토리에서 어느 `java` 를 실행하고 있는지는 `which java` 로 확인할 수 있습니다.

<br/>

### Download

```sh
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm

####################
### Java 7 - jdk7
####################
wget https://cdn.azul.com/zulu/bin/zulu7.34.0.5-ca-jdk7.0.242-linux_x64.tar.gz

####################
### Java 8 - jdk8
####################
wget https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jdk8.0.232-linux_x64.tar.gz

####################
### Java 8 - jre8
####################
wget https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jre8.0.232-linux_x64.tar.gz

####################
### Java 11 - jdk11
####################
wget https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz

####################
### Java 11 - jre11
####################
wget https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jre11.0.5-linux_x64.tar.gz
```

<br/>

### 압축해제

```sh
sudo mkdir /usr/lib/jvm
tar -zxvf zulu7.34.0.5-ca-jdk7.0.242-linux_x64.tar.gz
tar -zxvf zulu8.42.0.21-ca-jdk8.0.232-linux_x64.tar.gz
tar -zxvf zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz
```

<br/>

### profile 수정

`/etc/profile` : 모든 계정에 공통적으로 적용.

`.profile` : 접속한 로그인 계정을 대상으로 적용.

```sh
sudo vi /etc/profile
```

```sh
...(생략)
JAVA_HOME=/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64
JRE_HOME=/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64
PATH=$PATH:$JRE_HOME/bin:$JAVA_HOME/bin

export JAVA_HOME
export JRE_HOME
export PATH
```

```sh
### 설정적용
source /etc/profile
```

<br/>

### update-alternatives 설정

```sh
### 현재 update-alternatives 를 사용하고있는 모든 요소들을 보여준다.
### 아무내용을 입력하지 않고 Enter를치면 다음목록이 나온다.
update-alternatives --all
```

```sh
### editor로 묶여있는 파일목록을 확인할 수 있다.
### * 은 현재 선택되어있는 프로그램이다.
### 변경을 하지 않을거면 그냥 Enter 변경을 할거면 번호를입력한다.
update-alternatives --config editor
update-alternatives --config vi
update-alternatives --config java
```

```sh
#############################
### Java 7
### 심볼릭링크 생성(--install)
#############################
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/zulu7.34.0.5-ca-jdk7.0.242-linux_x64/bin/java" 7
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/zulu7.34.0.5-ca-jdk7.0.242-linux_x64/bin/javac" 7
#############################
### Java 7
### 심볼릭링크 변경(--set)
#############################
sudo update-alternatives --set java /usr/lib/jvm/zulu7.34.0.5-ca-jdk7.0.242-linux_x64/bin/java
sudo update-alternatives --set javac /usr/lib/jvm/zulu7.34.0.5-ca-jdk7.0.242-linux_x64/bin/javac
```

```sh
#############################
### Java 8
### 심볼릭링크 생성(--install)
#############################
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64/bin/java" 8
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64/bin/javac" 8
#############################
### Java 8
### 심볼릭링크 변경(--set)
#############################
sudo update-alternatives --set java /usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64/bin/java
sudo update-alternatives --set javac /usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64/bin/javac
```

```sh
#############################
### Java 11
### 심볼릭링크 생성(--install)
#############################
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/java" 11
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/javac" 11
#############################
### Java 11
### 심볼릭링크 변경(--set)
#############################
sudo update-alternatives --set java /usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/java
sudo update-alternatives --set javac /usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/binjavac
```

```sh
############################
### 심볼릭링크 제거(--remove)
############################
update-alternatives --remove <name> 경로명/파일명
sudo update-alternatives --remove java /usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/java
```

<br/>

### 확인

```sh
root@ubuntu-deploy:/usr/bin# java -version
openjdk version "1.8.0_232"
OpenJDK Runtime Environment (Zulu 8.42.0.21-CA-linux64) (build 1.8.0_232-b18)
OpenJDK 64-Bit Server VM (Zulu 8.42.0.21-CA-linux64) (build 25.232-b18, mixed mode)
root@ubuntu-deploy:/usr/bin#
```
