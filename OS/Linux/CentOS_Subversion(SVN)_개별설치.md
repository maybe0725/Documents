# CentOS*Subversion(SVN)*개별설치

<br/>

[참조글 링크](https://myjamong.tistory.com/12)

외부망 연결 가능 서버 : wget

외부망 연결 불가 서버 : 로컬PC 로 다운로드 받은 후 USB 옮겨서 작업

<br/>

## 1. SVN(Subversion), apr, apr-util, sqlite Download

```sh
[svn@maybe-test ~]$ mkdir /home/svn/downloads
[svn@maybe-test ~]$ cd /home/svn/downloads

### Subversion Download
[svn@maybe-test downloads]$ wget http://apache.mirror.cdnetworks.com/subversion/subversion-1.12.2.tar.gz

### apr Download
[svn@maybe-test downloads]$ wget https://www.apache.org/dist/apr/apr-1.7.0.tar.gz

### apr-util Download
[svn@maybe-test downloads]$ wget https://www.apache.org/dist/apr/apr-util-1.6.1.tar.gz

### SQLite : https://www.sqlite.org/download.html
[svn@maybe-test downloads]$ wget https://www.sqlite.org/2019/sqlite-autoconf-3300100.tar.gz
```

<br/>

## 2. 설치

```sh
### SVN(Subversion) 압축풀기
[svn@maybe-test downloads]$ mkdir /app/svn
[svn@maybe-test downloads]$ tar xvfz subversion-1.12.2.tar.gz -C /app/svn

### apr 압축풀기
[svn@maybe-test downloads]$ tar xvfz apr-1.7.0.tar.gz -C /app/svn

### apr-util 압축풀기
[svn@maybe-test downloads]$ tar xvfz apr-util-1.6.1.tar.gz -C /app/svn

[svn@maybe-test downloads]$ cd /app/svn
[svn@maybe-test svn]$ ll
drwxr-xr-x 27 svn svn 4096  4월  1  2019 apr-1.7.0
drwxr-xr-x 20 svn svn 4096 10월 18  2017 apr-util-1.6.1
drwxr-xr-x  6 svn svn 4096  7월 19 06:42 subversion-1.12.2

### apr 컴파일 설치
[svn@maybe-test svn]$ cd apr-1.7.0/
[svn@maybe-test apr-1.7.0]$ ./configure --prefix=/app/svn/apr
[svn@maybe-test apr-1.7.0]$ make
[svn@maybe-test apr-1.7.0]$ make install
[svn@maybe-test apr-1.7.0]$ make clean
### apr-util 컴파일 설치
```
