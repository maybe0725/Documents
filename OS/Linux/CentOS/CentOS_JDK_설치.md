# CentOS JDK 설치

<br/>

참고 - [zetawiki :: CentOS JDK 설치](https://zetawiki.com/wiki/CentOS_JDK_%EC%84%A4%EC%B9%98)

CentOS 8에서 테스트하였습니다.

<br/>

## 1. 개요.

- 리눅스에서도 JDK와 JRE는 별도의 패키지이다.<br/>
java-버전-openjdk 패키지가 JRE,<br/>
java-버전-openjdk-devel 패키지가 JDK라고 생각하면 된다.

- JDK가 JRE에 의존성이 있다.[2]<br/>
yum으로 JDK를 설치하라고 하면 JRE를 먼저 설치한다.

<br/>

## 2. 설치가능 확인.

```bash
[root@maybe-test ~]# yum list java*jdk-devel
java-1.8.0-openjdk-devel.x86_64                                 1:1.8.0.222.b10-0.el8_0                                 AppStream
java-11-openjdk-devel.x86_64                                    1:11.0.4.11-0.el8_0                                     AppStream
```

→ 1.8, 11 버전이 설치 가능하다. <br/>
→ 여기서는 1.8 버전을 설치한다.

<br/>

## 3. 설치.

```bash
[root@maybe-test ~]# yum install java-1.8.0-openjdk-devel.x86_64
...(생략)
=================================================================================================================================
 꾸러미                                아키텍처         버전                                           리포지토리           크기
=================================================================================================================================
Installing:
 java-1.8.0-openjdk-devel              x86_64           1:1.8.0.222.b10-0.el8_0                        AppStream           9.7 M
종속성 설치:
 copy-jdk-configs                      noarch           3.7-1.el8                                      AppStream            27 k
 java-1.8.0-openjdk                    x86_64           1:1.8.0.222.b10-0.el8_0                        AppStream           297 k
 java-1.8.0-openjdk-headless           x86_64           1:1.8.0.222.b10-0.el8_0                        AppStream            32 M
 javapackages-filesystem               noarch           5.3.0-1.module_el8.0.0+11+5b8c10bd             AppStream            30 k
 ttmkfdir                              x86_64           3.0.9-54.el8                                   AppStream            62 k
 tzdata-java                           noarch           2019a-1.el8                                    AppStream           188 k
 xorg-x11-fonts-Type1                  noarch           7.5-19.el8                                     AppStream           522 k
 lksctp-tools                          x86_64           1.0.18-3.el8                                   BaseOS              100 k
Enabling module streams:
 javapackages-runtime                                   201801

거래 요약
=================================================================================================================================
설치  9 꾸러미

총 다운로드 크기 : 43 M
설치 크기 : 155 M
이게 괜찮습니까 [y / N] : y
...(생략)
```

<br/>

## 4. 설치확인.

```bash
[root@maybe-test ~]# rpm -qa java*jdk-devel
java-1.8.0-openjdk-devel-1.8.0.222.b10-0.el8_0.x86_64
```

```sh
[root@maybe-test ~]# javac -version
javac 1.8.0_222
```

## 5. Hello world 테스트.

```bash
[root@maybe-test ~]# echo "public class HelloWorld {" > HelloWorld.java
[root@maybe-test ~]# echo "  public static void main(String[] args) {" >> HelloWorld.java
[root@maybe-test ~]# echo "    System.out.println(\"Hello, World\");" >> HelloWorld.java
[root@maybe-test ~]# echo "  }" >> HelloWorld.java
[root@maybe-test ~]# echo "}" >> HelloWorld.java
[root@maybe-test ~]# javac HelloWorld.java
[root@maybe-test ~]# java HelloWorld
Hello, World
[root@maybe-test ~]# rm -f HelloWorld.java HelloWorld.class
```
