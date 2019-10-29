# Ubuntu 18.04 Maven 수동설치

<br/>

[Apache Maven Web Site]

- https://www.apache.org/

- http://maven.apache.org/

- http://maven.apache.org/download.cgi

<br/>

[Apache Maven Download Link]

http://mirror.navercorp.com/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz

<br/>

### Maven Donwload

```sh
cd /app/downloads
wget http://mirror.navercorp.com/apache/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz
```

<br/>

### 압축해제

```sh
cd /app/downloads
tar -zxvf apache-maven-3.6.2-bin.tar.gz
```

<br/>

### 설치확인

```sh
mv apache-maven-3.6.2 /app
cd /app/apache-maven-3.6.2/bin
./mvn -version
```

<br/>

### 환경변수 등록 및 확인

```sh
cd /home/jenkins
vi .profile
```

```sh
JAVA_HOME=/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64
JRE_HOME=/usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64
MAVEN_HOME=/app/apache-maven-3.6.2
PATH=$PATH:$JRE_HOME/bin:$JAVA_HOME/bin:$MAVEN_HOME/bin

export JAVA_HOME
export JRE_HOME
export MAVEN_HOME
export PATH
```

```sh
jenkins@ubuntu-deploy:~$ mvn -version
Apache Maven 3.6.2 (40f52333136460af0dc0d7232c0dc0bcf0d9e117; 2019-08-27T15:06:16Z)
Maven home: /app/apache-maven-3.6.2
Java version: 1.8.0_232, vendor: Azul Systems, Inc., runtime: /usr/lib/jvm/zulu8.42.0.21-ca-jdk8.0.232-linux_x64/jre
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "4.15.0-66-generic", arch: "amd64", family: "unix"
jenkins@ubuntu-deploy:~$
```

<br/>
