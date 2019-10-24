# Ubuntu 18.04 OpenJDK 설치

<br/>

참고글 : [LINE의 OpenJDK 적용기: 호환성 확인부터 주의 사항까지](https://engineering.linecorp.com/ko/blog/line-open-jdk/)

참고글 : [OpenJdk 설치, 삭제, 업데이트](https://daddyprogrammer.org/post/2062/openjdk-install-update-delete/)

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

- https://adoptopenjdk.net/
- https://adoptopenjdk.net/releases.html
- https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u232-b09/OpenJDK8U-jdk_x64_linux_hotspot_8u232b09.tar.gz
- https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u232-b09/OpenJDK8U-jre_x64_linux_hotspot_8u232b09.tar.gz
- https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.5%2B10/OpenJDK11U-jdk_x64_linux_hotspot_11.0.5_10.tar.gz
- https://github.com/AdoptOpenJDK/openjdk11-binaries/releases/download/jdk-11.0.5%2B10/OpenJDK11U-jre_x64_linux_hotspot_11.0.5_10.tar.gz

- https://jdk.java.net/archive/
- https://download.java.net/java/GA/jdk11/9/GPL/openjdk-11.0.2_linux-x64_bin.tar.gz
- https://download.java.net/java/GA/jdk11/13/GPL/openjdk-11.0.1_linux-x64_bin.tar.gz
- https://download.java.net/java/ga/jdk11/openjdk-11_linux-x64_bin.tar.gz

- https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jdk8.0.232-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu8.42.0.21-ca-jre8.0.232-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz
- https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jre11.0.5-linux_x64.tar.gz

<br/>

### 자바 홈 폴더 JAVA HOME

설치방법에 따라 다르지만, yum으로 설치했다면 일반적으로 `/usr/lib/jvm/java` 디렉토리에 설치가 된다.

수동구성시 `/usr/java/jdk1.6.0_26` 과 같은 형식으로 하는 경우도 있는데, 이때도 `심볼릭링크`를 잡아주면 별 차이는 없다.

```sh
[root@zetawiki ~]# ll /usr/lib/jvm/java
lrwxrwxrwx. 1 root root 26 Feb  3 00:16 /usr/lib/jvm/java -> /etc/alternatives/java_sdk
```

```sh
[root@zetawiki ~]# ll /etc/alternatives/java_sdk
lrwxrwxrwx. 1 root root 38 Feb  3 00:16 /etc/alternatives/java_sdk -> /usr/lib/jvm/java-1.7.0-openjdk.x86_64
```

```sh
[root@zetawiki ~]# ll /usr/lib/jvm/java/
total 20
drwxr-xr-x. 2 root root 4096 Feb  3 00:16 bin
drwxr-xr-x. 3 root root 4096 Feb  3 00:16 include
drwxr-xr-x. 4 root root 4096 Feb  3 00:16 jre
drwxr-xr-x. 2 root root 4096 Feb  3 00:16 lib
drwxr-xr-x. 2 root root 4096 Feb  3 00:16 tapset
```

<br/>

### Download

```sh
sudo mkdir /usr/lib/jvm
cd /usr/lib/jvm
wget https://cdn.azul.com/zulu/bin/zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz
```

<br/>

### 압축해제

```sh
sudo mkdir /usr/lib/jvm
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
JAVA_HOME=/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64
#JRE_HOME=/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64
#PATH=$PATH:$JRE_HOME/bin:$JAVA_HOME/bin
PATH=$PATH:$JAVA_HOME/bin

export JAVA_HOME
#export JRE_HOME
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
```

```sh
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/java" 1
sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/javac" 1
sudo update-alternatives --set java /usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/java
sudo update-alternatives --set javac /usr/lib/jvm/zulu11.35.13-ca-jdk11.0.5-linux_x64/bin/javac
```

<br/>

### 확인

```sh
maybe@ubuntu-deploy:~$ java -version
openjdk version "11.0.5" 2019-10-15 LTS
OpenJDK Runtime Environment Zulu11.35+13-CA (build 11.0.5+10-LTS)
OpenJDK 64-Bit Server VM Zulu11.35+13-CA (build 11.0.5+10-LTS, mixed mode)
maybe@ubuntu-deploy:~$
```
