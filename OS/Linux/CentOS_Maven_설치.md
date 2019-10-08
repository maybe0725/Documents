# CentOS Maven 설치

<br/>

## 1. yum 을 이용한 maven 설치 방법

<br/>

### 1.1. Maven 설치

```sh
[maybe@maybe-test ~]$ yum list maven
[maybe@maybe-test ~]$ yum install -y maven
[maybe@maybe-test ~]$ mvn -version
Apache Maven 3.5.4 (Red Hat 3.5.4-5)
Maven home: /usr/share/maven
Java version: 1.8.0_222, vendor: Oracle Corporation, runtime: /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.222.b10-0.el8_0.x86_64/jre
Default locale: ko_KR, platform encoding: UTF-8
OS name: "linux", version: "4.18.0-80.el8.x86_64", arch: "amd64", family: "unix"
[maybe@maybe-test ~]$
```

<br/>

### 1.2. Maven 삭제

```sh
# 설치 확인
[maybe@maybe-test ~]$ yum list installed maven

# 삭제
[maybe@maybe-test ~]$ yum remove maven

# 재확인
[maybe@maybe-test ~]$ yum list installed maven
```

<br/>

## 2. maven download 및 설정

<br/>

### 2.1. Maven Download

```sh
[maybe@maybe-test ~]$ wget http://apache.mirror.cdnetworks.com/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz
--2019-10-08 00:40:51--  http://apache.mirror.cdnetworks.com/maven/maven-3/3.6.2/binaries/apache-maven-3.6.2-bin.tar.gz
Resolving apache.mirror.cdnetworks.com (apache.mirror.cdnetworks.com)... 14.0.101.165
Connecting to apache.mirror.cdnetworks.com (apache.mirror.cdnetworks.com)|14.0.101.165|:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 9142315 (8.7M) [application/x-gzip]
Saving to: ‘apache-maven-3.6.2-bin.tar.gz’

apache-maven-3.6.2-bin.tar.gz    100%[=======================================================>]   8.72M  15.7MB/s    in 0.6s

2019-10-08 00:40:52 (15.7 MB/s) - ‘apache-maven-3.6.2-bin.tar.gz’ saved [9142315/9142315]

[maybe@maybe-test ~]$ ll
합계 8932
-rw-rw-r--  1 maybe maybe 9142315  9월  2 17:43 apache-maven-3.6.2-bin.tar.gz
[maybe@maybe-test ~]$
```

<br/>

### 2.2. 압축해제

```sh
[maybe@maybe-test ~]$ tar -xf apache-maven-3.6.2-bin.tar.gz
[maybe@maybe-test ~]$ ll
합계 8932
drwxrwxr-x  6 maybe maybe      99 10월  8 00:47 apache-maven-3.6.2
-rw-rw-r--  1 maybe maybe 9142315  9월  2 17:43 apache-maven-3.6.2-bin.tar.gz
```

<br/>

### 2.3. 디렉토리 이름 변경 및 설치 파일 삭제

```sh
[maybe@maybe-test ~]$ mv apache-maven-3.6.2/ apache-maven/
[maybe@maybe-test ~]$ rm -f apache-maven-3.6.2-bin.tar.gz
[maybe@maybe-test ~]$ ll
합계 0
drwxrwxr-x  6 maybe maybe 99 10월  8 00:47 apache-maven
```
