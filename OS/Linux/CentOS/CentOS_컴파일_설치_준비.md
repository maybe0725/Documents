# CentOS 컴파일 설치 준비

<br/>

[참조글 링크](https://zetawiki.com/wiki/CentOS_%EC%BB%B4%ED%8C%8C%EC%9D%BC_%EC%84%A4%EC%B9%98_%EC%A4%80%EB%B9%84)

<br/>

## 1 개요

리눅스 컴파일 설치 준비

> 리눅스에서 컴파일 설치를 하려면 gcc, make 등이 필요하다.

<br/>

## 2 패키지 설치

```sh
[root@maybe-test ~]# yum install gcc make gcc-c++
마지막 메타 데이터 만료 확인 : 1:27:27 전에 2019년 10월 17일 (목) 오전 01시 01분 10초.
종속성이 해결되었습니다.
========================================================================================================================================
 꾸러미                            아키텍처                 버전                                      리포지토리                   크기
========================================================================================================================================
Installing:
 gcc                               x86_64                   8.2.1-3.5.el8                             AppStream                    23 M
 gcc-c++                           x86_64                   8.2.1-3.5.el8                             AppStream                    12 M
 make                              x86_64                   1:4.2.1-9.el8                             BaseOS                      498 k
종속성 설치:
 cpp                               x86_64                   8.2.1-3.5.el8                             AppStream                    10 M
 isl                               x86_64                   0.16.1-6.el8                              AppStream                   841 k
 libstdc++-devel                   x86_64                   8.2.1-3.5.el8                             AppStream                   2.0 M
 glibc-devel                       x86_64                   2.28-42.el8.1                             BaseOS                      1.0 M
 glibc-headers                     x86_64                   2.28-42.el8.1                             BaseOS                      465 k
 kernel-headers                    x86_64                   4.18.0-80.11.2.el8_0                      BaseOS                      1.6 M
 libxcrypt-devel                   x86_64                   4.1.1-4.el8                               BaseOS                       25 k

거래 요약
========================================================================================================================================
설치  10 꾸러미

총 다운로드 크기 : 52 M
설치 크기 : 140 M
이게 괜찮습니까 [y / N] : y
패키지 다운로드중:
(1/10): gcc-c++-8.2.1-3.5.el8.x86_64.rpm                                                                6.0 MB/s |  12 MB     00:02
(2/10): isl-0.16.1-6.el8.x86_64.rpm                                                                     5.5 MB/s | 841 kB     00:00
(3/10): gcc-8.2.1-3.5.el8.x86_64.rpm                                                                    9.7 MB/s |  23 MB     00:02
(4/10): libstdc++-devel-8.2.1-3.5.el8.x86_64.rpm                                                        8.9 MB/s | 2.0 MB     00:00
(5/10): glibc-headers-2.28-42.el8.1.x86_64.rpm                                                           63 kB/s | 465 kB     00:07
(6/10): glibc-devel-2.28-42.el8.1.x86_64.rpm                                                             64 kB/s | 1.0 MB     00:16
(7/10): libxcrypt-devel-4.1.1-4.el8.x86_64.rpm                                                          101 kB/s |  25 kB     00:00
(8/10): make-4.2.1-9.el8.x86_64.rpm                                                                      62 kB/s | 498 kB     00:08
(9/10): kernel-headers-4.18.0-80.11.2.el8_0.x86_64.rpm                                                   76 kB/s | 1.6 MB     00:21
[MIRROR] cpp-8.2.1-3.5.el8.x86_64.rpm: Curl error (28): Timeout was reached for http://mirror.navercorp.com/centos/8.0.1905/AppStream/x86_64/os/Packages/cpp-8.2.1-3.5.el8.x86_64.rpm [Operation too slow. Less than 1000 bytes/sec transferred the last 30 seconds]
(10/10): cpp-8.2.1-3.5.el8.x86_64.rpm                                                                   270 kB/s |  10 MB     00:39
----------------------------------------------------------------------------------------------------------------------------------------
합계                                                                                                    1.2 MB/s |  52 MB     00:42
트랜잭션 점검 실행 중
트랜잭션 검사가 성공했습니다.
트랜잭션 테스트 실행 중
트랜잭션 테스트가 완료되었습니다.
거래 실행 중
  준비 중입니다  :                                                                                                                  1/1
  Installing     : kernel-headers-4.18.0-80.11.2.el8_0.x86_64                                                                      1/10
  스크립틀릿 실행: glibc-headers-2.28-42.el8.1.x86_64                                                                              2/10
  Installing     : glibc-headers-2.28-42.el8.1.x86_64                                                                              2/10
  Installing     : libxcrypt-devel-4.1.1-4.el8.x86_64                                                                              3/10
  Installing     : glibc-devel-2.28-42.el8.1.x86_64                                                                                4/10
  스크립틀릿 실행: glibc-devel-2.28-42.el8.1.x86_64                                                                                4/10
  Installing     : libstdc++-devel-8.2.1-3.5.el8.x86_64                                                                            5/10
  Installing     : isl-0.16.1-6.el8.x86_64                                                                                         6/10
  스크립틀릿 실행: isl-0.16.1-6.el8.x86_64                                                                                         6/10
  Installing     : cpp-8.2.1-3.5.el8.x86_64                                                                                        7/10
  스크립틀릿 실행: cpp-8.2.1-3.5.el8.x86_64                                                                                        7/10
  Installing     : gcc-8.2.1-3.5.el8.x86_64                                                                                        8/10
  스크립틀릿 실행: gcc-8.2.1-3.5.el8.x86_64                                                                                        8/10
  Installing     : gcc-c++-8.2.1-3.5.el8.x86_64                                                                                    9/10
  Installing     : make-1:4.2.1-9.el8.x86_64                                                                                      10/10
  스크립틀릿 실행: make-1:4.2.1-9.el8.x86_64                                                                                      10/10
  확인 중        : cpp-8.2.1-3.5.el8.x86_64                                                                                        1/10
  확인 중        : gcc-8.2.1-3.5.el8.x86_64                                                                                        2/10
  확인 중        : gcc-c++-8.2.1-3.5.el8.x86_64                                                                                    3/10
  확인 중        : isl-0.16.1-6.el8.x86_64                                                                                         4/10
  확인 중        : libstdc++-devel-8.2.1-3.5.el8.x86_64                                                                            5/10
  확인 중        : glibc-devel-2.28-42.el8.1.x86_64                                                                                6/10
  확인 중        : glibc-headers-2.28-42.el8.1.x86_64                                                                              7/10
  확인 중        : kernel-headers-4.18.0-80.11.2.el8_0.x86_64                                                                      8/10
  확인 중        : libxcrypt-devel-4.1.1-4.el8.x86_64                                                                              9/10
  확인 중        : make-1:4.2.1-9.el8.x86_64                                                                                      10/10

설치됨:
  gcc-8.2.1-3.5.el8.x86_64                  gcc-c++-8.2.1-3.5.el8.x86_64              make-1:4.2.1-9.el8.x86_64
  cpp-8.2.1-3.5.el8.x86_64                  isl-0.16.1-6.el8.x86_64                   libstdc++-devel-8.2.1-3.5.el8.x86_64
  glibc-devel-2.28-42.el8.1.x86_64          glibc-headers-2.28-42.el8.1.x86_64        kernel-headers-4.18.0-80.11.2.el8_0.x86_64
  libxcrypt-devel-4.1.1-4.el8.x86_64

완료되었습니다!
[root@maybe-test ~]#
```
