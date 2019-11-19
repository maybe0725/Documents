# CentOS jenkins.war 설치.

<br/>

## 1. jenkins 계정 만들기

```sh
### 계정이 있는지 확인.
### cat /etc/passwd | grep 계정명

[root@maybe-test /]# cat /etc/passwd | grep jenkins
### → 결과 없음. 즉 testuser 계정 없음.

### 홈폴더 + 쉘환경 지정
###
### Case 01. 우분투, SUSE, Arch의 경우
### useradd 계정명 -m -s /bin/bash
### → -m 옵션을 명시해야 홈 디렉토리가 생성됨
### → -s /bin/bash 옵션을 명시해야 쉘 환경이 설정됨
###
### Case 02. CentOS
### useradd 계정명
### → CentOS 등 레드햇 계열에서는 아무 옵션을 주지 않아도 홈 디렉토리 생성되고 쉘 환경이 설정됨

[root@maybe-test /]# useradd jenkins
[root@maybe-test /]# cat /etc/passwd | grep jenkins
jenkins:x:1001:1001::/home/jenkins:/bin/bash
### → testuser 계정을 만들었다. UID와 GID는 500, 홈폴더는 /home/testuser 이고, bash 셸 사용이 가능하다.

[root@maybe-test /]# echo 'P@ssw0rd' | passwd --stdin jenkins
Changing password for user jenkins.
passwd: all authentication tokens updated successfully.
### → jenkins 계정의 패스워드를 P@ssw0rd 로 설정하였다.
```

<br/>

## 2. jenkins.war 설치

```sh
### jenkins.war 다운로드
[root@maybe-test /]# mkdir /app/jenkins
[root@maybe-test jenkins]# wget http://mirrors.jenkins.io/war-stable/2.190.1/jenkins.war
[root@maybe-test app]# chown -R jenkins:jenkins jenkins/

### jenkins start shell 만들기
[root@maybe-test jenkins]# vi /app/jenkins/start-jenkins.sh

#/bin/sh

echo 'jenkins.war start...'
nohup java -jar /app/jenkins/jenkins.war --httpPort=9090 > /app/logs/jenkins/jenkins.log 2>&1&
echo 'jenkins.war start... success running...'

[root@maybe-test jenkins]# chmod 755 start-jenkins.sh

### jenkins stop shell 만들기
[root@maybe-test jenkins]# vi /app/jenkins/stop-jenkins.sh

#/bin/sh

echo 'jenkins.war kill...'
kill -9 `ps -ef | grep jenkins.war | awk '{print $2}'`
echo 'jenkins.war kill... success'

[root@maybe-test jenkins]# chmod 755 stop-jenkins.sh
```

<br/>

## 3. jenkins 실행 및 중지

```sh
### jenkins 실행
[jenkins@maybe-test jenkins]$ ./start-jenkins.sh

### jenkins 중지
[jenkins@maybe-test jenkins]$ ./stop-jenkins.sh
```
