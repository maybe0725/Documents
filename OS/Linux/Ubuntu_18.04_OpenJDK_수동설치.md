# Ubuntu 18.04 OpenJDK 수동설치

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

### 1. Download

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

### 2. 압축해제

```sh
sudo mkdir /usr/lib/jvm
tar -zxvf zulu7.34.0.5-ca-jdk7.0.242-linux_x64.tar.gz
tar -zxvf zulu8.42.0.21-ca-jdk8.0.232-linux_x64.tar.gz
tar -zxvf zulu11.35.13-ca-jdk11.0.5-linux_x64.tar.gz
```

<br/>

### 3. profile 수정

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

### 4. 확인

```sh
root@ubuntu-deploy:/usr/bin# java -version
openjdk version "1.8.0_232"
OpenJDK Runtime Environment (Zulu 8.42.0.21-CA-linux64) (build 1.8.0_232-b18)
OpenJDK 64-Bit Server VM (Zulu 8.42.0.21-CA-linux64) (build 25.232-b18, mixed mode)
root@ubuntu-deploy:/usr/bin#
```
