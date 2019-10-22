# ssh 서버 설치.

<br/>

```sh
### 1. 설치확인
root@ubuntu:~# netstat -ntlp | grep sshd
root@ubuntu:~# service ssh status
Unit ssh.service could not be found.
### → 설치 안됨

root@ubuntu:~# dpkg --get-selections | grep ssh
openssh-client					install
### → openssh-client만 설치되어 있다. openssh-server가 필요하다.

### 2. 설치 및 시작
root@ubuntu:~# apt-get install openssh-server
...(생략)
root@ubuntu:~# service ssh restart

### 3. 확인
root@ubuntu:~# dpkg --get-selections | grep ssh
openssh-client					install
openssh-server					install
openssh-sftp-server     install
ssh-import-id					  install
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# service ssh status
ssh start/running, process 1702
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# ps -ef | grep sshd | grep -v grep
root      1702     1  0 03:53 ?        00:00:00 /usr/sbin/sshd -D
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# netstat -ntlp | grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1702/sshd
tcp6       0      0 :::22                   :::*                    LISTEN      1702/sshd

### 4. ssh root 계정 접속 허용 설정
root@ubuntu:~# vi /etc/ssh/sshd_config
### PermitRootLogin prohibit_password 주석 해제 후
### PermitRootLogin yes 로 수정.
### ssh restart
root@ubuntu:~# service ssh restart
```
