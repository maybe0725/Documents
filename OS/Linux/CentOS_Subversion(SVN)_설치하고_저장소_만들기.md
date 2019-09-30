# CentOS Subversion(SVN) 설치하고 저장소 만들기.

<br/>

## 1. CentOS Subversion 설치

<br/>

### 설치 할 수 있는 Subversion 을 확인
```sh
[root@maybe-test ~]# yum list subversion
```

### Subversion 설치
```sh
[root@maybe-test ~]# yum install -y subversion
```

### 설치확인
```sh
[root@maybe-test ~]# svn --version
```
또는
```sh
[root@maybe-test ~]# rpm -qa | grep subversion
```

<br/>

## 2. Subversion 저장소들을 저장할 폴더 생성 및 설정.

<br/>

### Subversion 에서 원격저장소를 저장할 최상위 폴더 생성
```sh
[root@maybe-test ~]# cd /
[root@maybe-test /]# cd mkdir svn_repos
```

### svnserve 파일에 방금 생성한 svn 저장소를 관리할 폴더를 지정.
### svnserve 파일을 생성해야 service svnserve start, stop, restart 가능
```sh
[root@maybe-test svn_repos]# vi /etc/sysconfig/svnserve
```
```sh
OPTIONS="--threads --root /svn_repos"
```
또는
```sh
[root@maybe-test svn_repos]# echo 'OPTIONS="--threads --root /svn_repos"' > /etc/sysconfig/svnserve
[root@maybe-test svn_repos]# cat /etc/sysconfig/svnserve
OPTIONS="--threads --root /svn_repos"
```

<br/>

## 3. Subversion 서비스 포트 방화벽 해제 설정

<br/>

```sh
[root@maybe-test svn_repos]# firewall-cmd --permanent --add-port=3690/tcp
[root@maybe-test svn_repos]# firewall-cmd --reload
```
또는
```sh
[root@maybe-test svn_repos]# firewall-cmd --permanent --zone=public --add-port=3690/tcp
[root@maybe-test svn_repos]# firewall-cmd --reload
```

<br/>

## 4. Subversion 실행

<br/>

### 실행
```sh
[root@maybe-test svn_repos]# systemctl start svnserve.service
```
### 중지
```sh
[root@maybe-test svn_repos]# systemctl stop svnserve.service
```
### 재실행
```sh
[root@maybe-test svn_repos]# systemctl restart svnserve.service
```
### SVN 서비스 실행 및 확인
```sh
[root@maybe-test svn_repos]# ps -efl | grep svn
1 S root      3152     1  0  80   0 - 65875 -      00:36 ?        00:00:00 /usr/bin/svnserve --daemon --pid-file=/run/svnserve/svnserve.pid --threads --root /etc/subversion/repos
0 S root      3548  2907  0  80   0 -  2637 -      01:18 pts/0    00:00:00 grep --color=auto svn
```
```sh
[root@maybe-test svn_repos]# netstat -anp | grep svnserve
tcp        0      0 0.0.0.0:3690            0.0.0.0:*               LISTEN      3152/svnserve
```

<br/>

## 5. Subversion 테스트 저장소 생성

<br/>

### 실제 프로젝트의 단위가 될 SVN 저장소 만들기
### svnadmin create --fs-type fsfs '저장소 명'
```sh
[root@maybe-test svn_repos]# svnadmin create --fs-type fsfs testproject
[root@maybe-test svn_repos]# ll
합계 0
drwxr-xr-x. 6 root root 86  9월 26 03:58 testproject
[root@maybe-test svn_repos]#
```
### 생성한 testproject 저장소의 svnserve.conf 파일 수정
```sh
[root@maybe-test svn_repos]# cd /svn_repos/testproject/conf
[root@maybe-test svn_repos]# cat svnserve.conf
[root@maybe-test svn_repos]# mv svnserve.conf svnserve.conf.old
[root@maybe-test svn_repos]# echo '[general]' > svnserve.conf
[root@maybe-test svn_repos]# echo 'anon-access = none' >> svnserve.conf
[root@maybe-test svn_repos]# echo 'auth-access = write' >> svnserve.conf
[root@maybe-test svn_repos]# echo 'password-db = passwd' >> svnserve.conf
[root@maybe-test svn_repos]# echo 'authz-db = authz' >> svnserve.conf
[root@maybe-test svn_repos]# cat svnserve.conf
[general]
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
```

### [참고]
- anon-access : 로그인 하지 않은 사용자(비인증 계정)에게 접근권한을 설정하는 부분<br/>
                read, write, none 세가지 값을 설정 할 수 있다.
- auth-access : 로그인한 사용자(인증계정)에 대한 접근 권한을 설정하는 부분.<br/>
                read, write, none 세가지 값을 설정 할 수 있다.
- password-db : 저장소에 접근할 사용자 계정과 비밀번호를 관리할 파일의 이름을 지정하는 설정이다.<br/>
                기본 파일명은 passwd 이며, 다른 이름을 사용할 수 있다.
- authz-db    : 파일과 디렉토리에 대한 접근 권한을 관리하는 파일의 이름을 지정하는 설정이다.<br/>
                기본 파일명은 authz 이며, 다른 이름을 사용할 수 있다.
- realm       : 인증할 때 보여주는 간단한 저장소 설명이며, 생략 가능하다.
- none        : 접근권한 없음
- read        : 읽기 권한
- write       : 쓰기 권한

<br/>

## 6. 저장소에 접근할 계정정보 추가

<br/>

### passwd 파일 수정
```sh
[root@maybe-test conf]# vi passwd
```
```sh
[users]
admin=admin
testuser1=test
testuser2=test
```

### authz 파일 수정
```sh
[root@maybe-test conf]# vi authz
```
```sh
[/]
*=r          # testproject 저장소의 루트경로에 모든 사용자가 read 할 수 있는 권한 부여.
root=rw      # testproject 저장소의 루트경로에 root      계정은 read, write 권한 부여.
admin=rw     # testproject 저장소의 루트경로에 admin     계정은 read, write 권한 부여.
testuser1=rw # testproject 저장소의 루트경로에 testuser1 계정은 read, write 권한 부여.
testuser2=rw # testproject 저장소의 루트경로에 testuser2 계정은 read, write 권한 부여.
```

### 설정 완료 후 svnserve 데몬을 재시작
```sh
[root@maybe-test conf]# systemctl restart svnserve.service
```

<br/>

## 7. SVN 저정소 확인

<br/>

```sh
[root@maybe-test testproject]# svn list svn://127.0.0.1:3690/testproject
인증 영역(realm): <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0
'testuser1'의 암호:****


-----------------------------------------------------------------------
주의! 인증정보 영역:

   <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0

에 대한 당신의 비밀번호는 디스크에 암호화되어 저장되지 않습니다.
가능하면, 비밀번호를 암호화하여 저장하도록 설정을 바꾸십시오.
자세한 것은 문서를 참조하세요.

이 주의 문구를 다음에 보이지 않게 하려면, 'store-plaintext-passwords'의
설정을 'yes' 혹은 'no'로 지정하면 됩니다. 설정 파일은 다음과 같습니다.
'/root/.subversion/servers'
-----------------------------------------------------------------------
비밀번호를 평문으로 저장하겠습니까 (yes/no)?n
BootTest/
[root@maybe-test testproject]#
```

```sg
[root@maybe-test testproject]# svn info svn://127.0.0.1:3690/testproject
인증 영역(realm): <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0
'testuser1'의 암호:****


-----------------------------------------------------------------------
주의! 인증정보 영역:

   <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0

에 대한 당신의 비밀번호는 디스크에 암호화되어 저장되지 않습니다.
가능하면, 비밀번호를 암호화하여 저장하도록 설정을 바꾸십시오.
자세한 것은 문서를 참조하세요.

이 주의 문구를 다음에 보이지 않게 하려면, 'store-plaintext-passwords'의
설정을 'yes' 혹은 'no'로 지정하면 됩니다. 설정 파일은 다음과 같습니다.
'/root/.subversion/servers'
-----------------------------------------------------------------------
비밀번호를 평문으로 저장하겠습니까 (yes/no)?n
경로: testproject
URL: svn://127.0.0.1/testproject
Relative URL: ^/
저장소 루트: svn://127.0.0.1/testproject
저장소 UUID: af9f80d4-61d3-44fe-92b0-43d0800205f0
리비전: 1
노드 종류: 디렉토리
마지막 수정 작업자: testuser1
마지막 수정 리비전: 1
마지막 수정 일자: 2019-09-30 00:40:50 -0400 (2019-09-30, 월)

[root@maybe-test testproject]#
```

SVN 인증 확인에 실패했다면, svnserve.conf, passwd, authz 설정 부분을 다시 확인 해야 합니다.

SELinux 가 활성화 되어 있는 경우, SVN 저장소 접근이 안됩니다.

이 경우, CentOS SeLinux 를 disabled 합니다.

sestatus 로 SELinux 의 상태를 확인 할 수 있습니다.

```sh
[root@maybe-test testproject]# sestatus
SELinux status:                 disabled
```

만약 활성화(enabled) 되어 있는 경우라면 `/etc/selinux/config` 파일을 수정 합니다.

```sh
[root@maybe-test testproject]# vi /etc/selinux/config
```

```sh
# This file controls the state of SELinux on the system.
# SELINUX= can take one of these three values:
#     enforcing - SELinux security policy is enforced.
#     permissive - SELinux prints warnings instead of enforcing.
#     disabled - No SELinux policy is loaded.
SELINUX=disabled
# SELINUXTYPE= can take one of these three values:
#     targeted - Targeted processes are protected,
#     minimum - Modification of targeted policy. Only selected processes are protected.
#     mls - Multi Level Security protection.
SELINUXTYPE=targeted
```

<br/>

## 8. SVN trunk, tags, branches 기본 디렉토리 만들기

<br/>

```sh
[root@maybe-test testproject]# svn mkdir svn://127.0.0.1:3690/testproject/trunk
```

svn mkdir 명령어를 사용할 수 없는 경우, 아래와 같은 메시지가 나옵니다.

```sh
[root@maybe-test testproject]# svn mkdir svn://127.0.0.1:3690/testproject/trunk
svn: E205007: 로그 메시지를 구하기 위해 외부 프로그램을 사용할 수 없습니다. SVN_EDITOR 환경변수를 설정하시거나 --message (-m) 또는 --file (-F) 옵션을 사용하세요
svn: E205007: 환경변수 SVN_EDITOR, VISUAL, EDITOR 중 하나는 설정하거나, 'editor-cmd' 를 구성화일에 명시해야합니다
[root@maybe-test testproject]#
```

이를 해결하기 위해서는

~./bash_profile 맨 아래에 내용을 추가해야 합니다.

```sh
[root@maybe-test testproject]# cd ~
[root@maybe-test ~]# vi .bash_profile
```

```sh
# Subversion
SVN_EDITOR=/usr/bin/vim
export SVN_EDITOR
```

저장 후

```sh
[root@maybe-test ~]# source .bash_profile
```

```sh
[root@maybe-test ~]# svn mkdir svn://127.0.0.1:3690/testproject/trunk

로그 메시지가 변경되지 않았거나 지정되지 않았습니다
취소(A), 계속(C), 수정(E):
c
인증 영역(realm): <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0
'testuser1'의 암호:****


-----------------------------------------------------------------------
주의! 인증정보 영역:

   <svn://127.0.0.1:3690> af9f80d4-61d3-44fe-92b0-43d0800205f0

에 대한 당신의 비밀번호는 디스크에 암호화되어 저장되지 않습니다.
가능하면, 비밀번호를 암호화하여 저장하도록 설정을 바꾸십시오.
자세한 것은 문서를 참조하세요.

이 주의 문구를 다음에 보이지 않게 하려면, 'store-plaintext-passwords'의
설정을 'yes' 혹은 'no'로 지정하면 됩니다. 설정 파일은 다음과 같습니다.
'/root/.subversion/servers'
-----------------------------------------------------------------------
비밀번호를 평문으로 저장하겠습니까 (yes/no)?n
Committing transaction...
커밋된 리비전 2.
[root@maybe-test ~]#
```

```sh
[root@maybe-test ~]# svn mkdir svn://127.0.0.1:3690/testproject/tags
...(생략)
[root@maybe-test ~]# svn mkdir svn://127.0.0.1:3690/testproject/branches
...(생략)
```

디렉토리(trunk, tags, branches)가 생성되었는지 확인

```sh
[root@maybe-test ~]# svn list svn://127.0.0.1:3690/testproject
branches/
tags/
trunk/
```

### CentOS vim 설치 (만약, vim 이 없다는 에러가 발생하는 경우) 

```sh
### yum을 이용하여 vim 설치
[root@maybe-test ~]# yum install vim-enhanced

### vim 설정
[root@maybe-test ~]# vi /etc/profile

alias vi='vim'
alias ll='ls -al'

### :wq 로 저장 후 source 를 이용해 프로필을 적용
[root@maybe-test ~]# source /etc/profile
```

<br/>

## 추가 1. 서버 동작 시 SVN 서비스 자동 실행하기

```sh
[root@maybe-test ~]# systemctl enable svnserve.service
```

<br/>

## 추가 2. 저장소 삭제

만들 때는 svnadmin 으로 생성했지만, 지울 때는 그냥 저장소 폴더를 지우면 됩니다.

```sh
[root@maybe-test ~]# service svnserve stop
[root@maybe-test ~]# rm -rf /svn_repos/testproject
```

<br/>

## 추가 3. SVN E220001 오류

Error: 220001 (Item is not readable) Description: Unreadable path encountered; access denied

SVN E220001 오류가 발생 했을 때 SVN Server 설정 파일 중에서

svnserve.conf 파일을 아래와 같이 설정

```
[general]
anon-access=none
```
