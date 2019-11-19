# Ubuntu 18.04 hostname 변경

<br/>

```sh
####################
### 1. hostname 확인
####################
root@ubuntu:~# hostnamectl
   Static hostname: ubuntu
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 6fe145885bc94d21a697be76bc6cbab1
           Boot ID: 6c5c562af3af4bcdb936c8b3c98b47b4
    Virtualization: oracle
  Operating System: Ubuntu 18.04.3 LTS
            Kernel: Linux 4.15.0-66-generic
      Architecture: x86-64
root@ubuntu:~#

####################
### 2. hostname 변경
####################
root@ubuntu:~# hostnamectl set-hostname ubuntu-deploy

#####################
### 3. server reboot
#####################
root@ubuntu:~# reboot

####################
### 4. hostname 확인
####################
root@ubuntu-deploy:~# hostnamectl
   Static hostname: ubuntu-deploy
         Icon name: computer-vm
           Chassis: vm
        Machine ID: 6fe145885bc94d21a697be76bc6cbab1
           Boot ID: 34a81647c52f45ad8034d21c5bd01df1
    Virtualization: oracle
  Operating System: Ubuntu 18.04.3 LTS
            Kernel: Linux 4.15.0-66-generic
      Architecture: x86-64
root@ubuntu-deploy:~#

###################################################################
### 5. hostname 이 변경이 안되는 경우 /etc/cloud/cloud.cfg 파일 수정.
###################################################################
root@ubuntu-deploy:~# vi /etc/cloud/cloud.cfg
...(생략)
#preserve_hostname: false
preserve_hostname: true
...(생략)
root@ubuntu-deploy:~#

############################
### 5.1. 로그인 서비스 재시작
############################
root@ubuntu-deploy:~# systemctl restart systemd-logind.service
```
