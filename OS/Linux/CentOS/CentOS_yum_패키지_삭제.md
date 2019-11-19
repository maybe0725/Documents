# CentOS yum 패키지 삭제

<br/>

참조링크 : https://zetawiki.com/wiki/CentOS_yum_%ED%8C%A8%ED%82%A4%EC%A7%80_%EC%82%AD%EC%A0%9C

<br/>

## 1 개요

yum 삭제

yum으로 삭제하기

> yum을 삭제하는 것이 아니라, yum으로 다른 패키지를 삭제하는 것

<br/>

## 2 설치 확인

```sh
[root@zetawiki ~]# yum list installed java*jdk
... (생략)
Installed Packages
java-1.7.0-openjdk.x86_64            1:1.7.0.9-2.3.3.el5.1             installed
```

→ java-1.7.0-openjdk.x86_64 패키지가 설치되어 있음

<br/>

## 3 삭제

```sh
[root@zetawiki ~]# yum remove java-1.7.0-openjdk.x86_64
... (생략)
================================================================================
 Package                    Arch     Version                  Repository   Size
================================================================================
Removing:
 java-1.7.0-openjdk         x86_64   1:1.7.0.9-2.3.3.el5.1    installed    52 M
Removing for dependencies:
 java-1.7.0-openjdk-devel   x86_64   1:1.7.0.9-2.3.3.el5.1    installed    26 M

Transaction Summary
================================================================================
Remove        2 Package(s)
Reinstall     0 Package(s)
Downgrade     0 Package(s)

Is this ok [y/N]:
```

y `↵ Enter`

```sh
... (생략)
Removed:
  java-1.7.0-openjdk.x86_64 1:1.7.0.9-2.3.3.el5.1

Dependency Removed:
  java-1.7.0-openjdk-devel.x86_64 1:1.7.0.9-2.3.3.el5.1

Complete!
```

<br/>

## 4 재확인

```sh
[root@zetawiki ~]# yum list installed java*jdk
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
Error: No matching Packages to list
```

→ 삭제되어 없음
