# CentOS Jenkins 설치.

<br/>

## 1. Jenkins 설치 

```sh
[root@maybe-test ~]# wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
[root@maybe-test ~]# rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io.key
[root@maybe-test ~]# yum install jenkins
```

<br/>

## 2. Jenkins 포트변경.

```sh
[root@maybe-test ~]# vi /etc/sysconfig/jenkins
...(생략)
# Jenkins 기본포트 8080을 9090 포트로 변경 #
JENKINS_PORT="9090"
...(생략)
```

<br/>

## 3. 방화벽 오픈

CentOS 버젼에 따라 아래 1) , 2) 방법으로 방화벽을 오픈한다.

### 3.1. CentOS 7 이상이면 firewall을 이용하여 방화벽 오픈 

```sh
[root@maybe-test ~]# firewall-cmd --permanent --add-port=9090/tcp 
[root@maybe-test ~]# firewall-cmd --reload
```

### 3.2. CentOS 7보다 낮으면 iptables을 수정하여 방화벽 오픈 

```sh
[root@maybe-test ~]# vi /etc/sysconfig/iptables
...(생략)
# 추가 #
- A INPUT -m state --state NEW -m tcp -p tcp --dport 9090 -j ACCEPT 
...(생략)

# iptables 재시작 #
[root@maybe-test ~]# service iptables restart 
```

<br/>

## 4. Jenkins 시작 및 설정

```sh
# jenkins 시작 #
[root@maybe-test ~]# service jenkins start
```

웹 브라우저로 접속 확인

localhost:9090 or ipAddress:9090

첫 페이지에서 Administrator password를 입력해야된다. 

```sh
[root@maybe-test ~]# vi /var/lib/jenkins/secrets/initialAdminPassword
```
해당 경로로 들어가면 초기 Administrator password가 있다. 복사 후 입력 후 'Continue' 버튼을 클릭, 다음 페이지로 이동.

Install suggested plugins 클릭하여 플러그인 설치.

다음으로는 관리자 계정 생성 부분이다 

각 항목을 입력 후 Save and Continue 클릭.

Jenkins 메인 페이지를 확인 할 수 있다.



