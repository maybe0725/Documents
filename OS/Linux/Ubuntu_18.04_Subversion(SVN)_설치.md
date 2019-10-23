# Ubuntu 18.04 Subversion(SVN) 설치

<br/>

## 1. 사전준비

### gcc 설치

```sh
###############
### 1. gcc 설치
###############
root@ubuntu-deploy:~# apt install gcc
```

<br/>

### make 설치

```sh
################
### 1. make 설치
################
root@ubuntu-deploy:/app/svn/apr-1.7.0$ apt install make
```

<br/>

## 2. APR(Apache Portable Runtime) 설치하기

<br/>

APR Download Link - https://www.apache.org/dist/apr/apr-1.7.0.tar.gz

```sh
###################
### 1. APR 다운로드
###################
svn@ubuntu-deploy:~$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ wget https://www.apache.org/dist/apr/apr-1.7.0.tar.gz

################
### 2. 압축 풀기
################
svn@ubuntu-deploy:/app/svn$ tar -zxvf apr-1.7.0.tar.gz

##################
### 3. 컴파일 설치
##################
svn@ubuntu-deploy:/app/svn$ cd apr-1.7.0
svn@ubuntu-deploy:/app/svn/apr-1.7.0$ ./configure --prefix=/app/svn/apr
svn@ubuntu-deploy:/app/svn/apr-1.7.0$ make
svn@ubuntu-deploy:/app/svn/apr-1.7.0$ make install
svn@ubuntu-deploy:/app/svn/apr-1.7.0$ make clean
```

<br/>

## 3. APR(Apache Portable Runtime) 설치하기

<br/>

```sh
########################
### 1. APR Util 다운로드
########################
svn@ubuntu-deploy:~$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ wget https://www.apache.org/dist/apr/apr-util-1.6.1.tar.gz

################
### 2. 압축 풀기
################
svn@ubuntu-deploy:/app/svn$ tar -zxvf apr-util-1.6.1.tar.gz

##################
### 3. 컴파일 설치
##################
svn@ubuntu-deploy:/app/svn$ cd apr-util-1.6.1
svn@ubuntu-deploy:/app/svn/apr-util-1.6.1$ ./configure --prefix=/app/svn/apr-util --with-apr=/app/svn/apr
svn@ubuntu-deploy:/app/svn/apr-util-1.6.1$ make
svn@ubuntu-deploy:/app/svn/apr-util-1.6.1$ make install
svn@ubuntu-deploy:/app/svn/apr-util-1.6.1$ make clean

####################################################
### 3.1. make 명령 실행시 에러 발생 조치 방법
### fatal error: expat.h: No such file or directory
### CentOS : yum install expat-devel
### Ubuntu : apt install libexpat1-dev
####################################################
```

<br/>

## 4. zlib 설치하기

<br/>

```sh
####################
### 1. zlib 다운로드
####################
svn@ubuntu-deploy:~$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ wget https://www.zlib.net/zlib-1.2.11.tar.gz

################
### 2. 압축 풀기
################
svn@ubuntu-deploy:/app/svn$ tar -zxvf zlib-1.2.11.tar.gz

##################
### 3. 컴파일 설치
##################
svn@ubuntu-deploy:/app/svn$ cd zlib-1.2.11
svn@ubuntu-deploy:/app/svn/zlib-1.2.11$ ./configure
svn@ubuntu-deploy:/app/svn/zlib-1.2.11$ make
root@ubuntu-deploy:/app/svn/zlib-1.2.11$ make install
root@ubuntu-deploy:/app/svn/zlib-1.2.11$ make clean
```

<br/>

## 5. sql-lite 설치하기

<br/>

```sh
########################
### 1. sql-lite 다운로드
########################
svn@ubuntu-deploy:~$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ wget https://www.sqlite.org/2019/sqlite-autoconf-3300100.tar.gz

################
### 2. 압축 풀기
################
svn@ubuntu-deploy:/app/svn$ tar -zxvf sqlite-autoconf-3300100.tar.gz
```

<br/>

## 6. Subversion(SVN) 설치하기

<br/>

```sh
###############################
### 1. Subversion(SVN) 다운로드
###############################
svn@ubuntu-deploy:~$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ wget http://apache.mirror.cdnetworks.com/subversion/subversion-1.12.2.tar.gz

################
### 2. 압축 풀기
################
svn@ubuntu-deploy:/app/svn$ tar -zxvf subversion-1.12.2.tar.gz

#####################################################################
### 3. sqlite3.c 파일을 SVN 소스 설치 폴더에 폴더를 만들어주고 넣어준다.
#####################################################################
svn@ubuntu-deploy:/app/svn$ cd subversion-1.12.2
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ mkdir sqlite-amalgamation
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ cd /app/svn/sqlite-autoconf-3300100
svn@ubuntu-deploy:/app/svn/sqlite-autoconf-3300100$ cp sqlite3.c /app/svn/subversion-1.12.2/sqlite-amalgamation

######################
### 4. SVN 컴파일 설치
######################
svn@ubuntu-deploy:~$ cd /app/svn/subversion-1.12.2
### 아래 3개의 명령어중 3번째 명령어 실행.
### command 1 : error
#svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ ./configure --prefix=/app/svn --with-apr=/app/svn/apr --with-apr-util=/app/svn/apr-util --without-berkeley-db
### command 2 : error
#svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ ./configure --prefix=/app/svn --with-apr=/app/svn/apr --with-apr-util=/app/svn/apr-util --without-berkeley-db --with-lz4=internal
### command 3 : success
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ ./configure --prefix=/app/svn --with-apr=/app/svn/apr --with-apr-util=/app/svn/apr-util --without-berkeley-db --with-lz4=internal --with-utf8proc=internal
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ make
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ make install
svn@ubuntu-deploy:/app/svn/subversion-1.12.2$ make clean


#############################################################
### 4.1. make 명령 실행시 에러 발생 조치 방법
### fatal error: sqlite3ext.h: No such file or directory
### fatal error: sqlite3.h: No such file or directory
### sqlite 디렉토리에서 sqlite3ext.h, sqlite3.h 두개 파일 복사
#############################################################
svn@ubuntu-deploy:/app/svn/sqlite-autoconf-3300100$ cp sqlite3ext.h /app/svn/subversion-1.12.2/sqlite-amalgamation
svn@ubuntu-deploy:/app/svn/sqlite-autoconf-3300100$ cp sqlite3.h /app/svn/subversion-1.12.2/sqlite-amalgamation
```

<br/>

## 7. Subversion(SVN) 설정

<br/>

### 저장소 생성

```sh
svn@ubuntu-deploy:/app/svn$ cd /app/svn
svn@ubuntu-deploy:/app/svn$ mkdir repository
svn@ubuntu-deploy:/app/svn$ cd bin
svn@ubuntu-deploy:/app/svn/bin$ ./svnadmin create --fs-type fsfs /app/svn/repository
```

<br/>

### 유저 및 비밀번호 등록

```sh
svn@ubuntu-deploy:/app/svn/repository/conf$ cd /app/svn/repository/conf
svn@ubuntu-deploy:/app/svn/repository/conf$ vi passwd
### This file is an example password file for svnserve.
### Its format is similar to that of svnserve.conf. As shown in the
### example below it contains one section labelled [users].
### The name and password for each user follow, one account per line.

[users]
# harry = harryssecret
# sally = sallyssecret
admin = admin1!
maybe = maybe1!
```

<br/>

### svnserve.conf 설정

```sh
svn@ubuntu-deploy:/app/svn/repository/conf$ cd /app/svn/repository/conf
svn@ubuntu-deploy:/app/svn/repository/conf$ vi svnserve.conf
...(생략)
[general]
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
...(생략)
```

```
[참고]
- anon-access : 로그인 하지 않은 사용자(비인증 계정)에게 접근권한을 설정하는 부분
                read, write, none 세가지 값을 설정 할 수 있다.
- auth-access : 로그인한 사용자(인증계정)에 대한 접근 권한을 설정하는 부분.
                read, write, none 세가지 값을 설정 할 수 있다.
- password-db : 저장소에 접근할 사용자 계정과 비밀번호를 관리할 파일의 이름을 지정하는 설정이다.
                기본 파일명은 passwd 이며, 다른 이름을 사용할 수 있다.
- authz-db    : 파일과 디렉토리에 대한 접근 권한을 관리하는 파일의 이름을 지정하는 설정이다.
                기본 파일명은 authz 이며, 다른 이름을 사용할 수 있다.
- realm       : 인증할 때 보여주는 간단한 저장소 설명이며, 생략 가능하다.
- none        : 접근권한 없음
- read        : 읽기 권한
- write       : 쓰기 권한
```

<br/>

### 권한설정

```sh
svn@ubuntu-deploy:/app/svn/repository/conf$ cd /app/svn/repository/conf
svn@ubuntu-deploy:/app/svn/repository/conf$ vi authz
...(생략)
[groups]
dev = admin,maybe

[/]
@dev = rw
...(생략)
```

<br/>

### 방화벽포트 개방

default port 3690.

```sh
root@ubuntu-deploy:/app/svn/repository/conf# firewall-cmd --list-ports

root@ubuntu-deploy:/app/svn/repository/conf# firewall-cmd --zone=public --add-port=3690/tcp --permanent
success
root@ubuntu-deploy:/app/svn/repository/conf# firewall-cmd --reload
success
root@ubuntu-deploy:/app/svn/repository/conf# firewall-cmd --list-ports
3690/tcp
root@ubuntu-deploy:/app/svn/repository/conf#
```

<br/>

### Subversion(SVN) 실행

```sh
svn@ubuntu-deploy:~$ /app/svn/bin/svnserve -d -r /app/svn
```

<br/>

### Subversion(SVN) Server 시작/종료를 위한 쉘 스크립트 작성

```sh
[시작 스크립트]
svn@ubuntu-deploy:/app/svn$ vi start.sh
#!/bin/bash
#/usr/local/bin/svnserve -d --threads -r [레파지토리 전체 경로]
/app/svn/bin/svnserve -d --threads -r /app/svn/repository


[종료 스크립트]
svn@ubuntu-deploy:/app/svn$ vi stop.sh
#!/bin/bash
ps -ef | grep svnserve | grep -v grep | awk '{print "kill -9", \$2}' | sh


[실행권한부여]
svn@ubuntu-deploy:/app/svn$ chmod +x start.sh
svn@ubuntu-deploy:/app/svn$ chmod +x stop.sh
```

<br/>

### 저장소 확인

```sh
svn@ubuntu-deploy:/app/svn/bin$ ./svn list svn://127.0.0.1:3690/repository
```

<br/>

### trunk, tags, branches 기본 디렉토리 만들기

SVN 기본 디렉토리 생성시 에러가 발생하는 경우는 아래 조치 방법 참고.

```sh
svn@ubuntu-deploy:/app/svn/bin$ ./svn mkdir svn://127.0.0.1:3690/repository/trunk

Log message unchanged or not specified
(a)bort, (c)ontinue, (e)dit:
c
Authentication realm: <svn://127.0.0.1:3690> 211e623c-f537-11e9-8050-711b1d607698
Password for 'maybe': *******

Committing transaction...
Committed revision 4.
svn@ubuntu-deploy:/app/svn/bin$
```

```sh
svn@ubuntu-deploy:/app/svn/bin$ ./svn mkdir svn://127.0.0.1:3690/repository/tags

Log message unchanged or not specified
(a)bort, (c)ontinue, (e)dit:
c
Authentication realm: <svn://127.0.0.1:3690> 211e623c-f537-11e9-8050-711b1d607698
Password for 'maybe': *******

Committing transaction...
Committed revision 5.
svn@ubuntu-deploy:/app/svn/bin$
```

```sh
svn@ubuntu-deploy:/app/svn/bin$ ./svn mkdir svn://127.0.0.1:3690/repository/branches

Log message unchanged or not specified
(a)bort, (c)ontinue, (e)dit:
c
Authentication realm: <svn://127.0.0.1:3690> 211e623c-f537-11e9-8050-711b1d607698
Password for 'maybe': *******

Committing transaction...
Committed revision 6.
svn@ubuntu-deploy:/app/svn/bin$
```

```sh
svn@ubuntu-deploy:/app/svn/bin$ ./svn list svn://127.0.0.1:3690/repository
Authentication realm: <svn://127.0.0.1:3690> 211e623c-f537-11e9-8050-711b1d607698
Password for 'maybe': *******

branches/
tags/
trunk/
svn@ubuntu-deploy:/app/svn/bin$
```

<br/>

### SVN 기본 디렉토리 생성시 에러발생 조치 방법

svn mkdir 명령어를 사용할 수 없는 경우, 아래와 같은 메시지가 나옵니다.

```
[ENG]]
svn: E205007: Could not use external editor to fetch log message; consider setting the $SVN_EDITOR environment variable or using the --message (-m) or --file (-F) options
svn: E205007: None of the environment variables SVN_EDITOR, VISUAL or EDITOR are set, and no 'editor-cmd' run-time configuration option was found

[KOR]
svn: E205007: 로그 메시지를 구하기 위해 외부 프로그램을 사용할 수 없습니다. SVN_EDITOR 환경변수를 설정하시거나 --message (-m) 또는 --file (-F) 옵션을 사용하세요
svn: E205007: 환경변수 SVN_EDITOR, VISUAL, EDITOR 중 하나는 설정하거나, 'editor-cmd' 를 구성화일에 명시해야합니다
```

```sh
####################################
### 조치 방법
### Ubuntu : .profile 파일 수정
### CentOS : .bash_profile 파일 수정
####################################
svn@ubuntu-deploy:~$ cd ~
svn@ubuntu-deploy:~$ vi .profile

#################
### 아래내용 추가
#################
# Subversion
export SVN_EDITOR=/usr/bin/vim

####################
### 내용추가 후 적용
####################
svn@ubuntu-deploy:~$ source .profile
```

<br/>

### 리눅스에서 source 명령어란 무엇일까 ?

source 명령어는 스크립트 파일을 수정한 후에 수정된 값을 바로 적용하기 위해 사용하는 명령어이다.

예륻들어 /etc/bashrc 파일을 수정 후 저장하여도 수정한 내용이 바로 적용되지 않는다.

왜냐하면 /etc/bashrc 파일은 유저가 로그인할 때 읽어들이는 파일이여서, 로그아웃 후 로그인하거나

리눅스를 재시작해야 적용이 된다.

이럴경우 간단하게 `# source /etc/bashrc` 명령어로 `수정된 내용을 바로 적용`할 수 있다.

<br/>
