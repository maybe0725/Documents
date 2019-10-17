# 리눅스 계정 생성 useradd

<br/>

## 1 계정이 있는지 확인

<br/>

cat /etc/passwd | grep 계정명

```sh
[root@maybe-edu ~]# cat /etc/passwd | grep testuser
```

→ 결과 없음. 즉 testuser 계정 없음.

<br/>

## 2 홈폴더 및 쉘환경 지정

<br/>

### 우분투, SUSE, Arch의 경우

```
useradd 계정명 -m -s /bin/bash
```

→ -m 옵션을 명시해야 홈 디렉토리가 생성됨
→ -s /bin/bash 옵션을 명시해야 쉘 환경이 설정됨

### CentOS

```
useradd 계정명
```

→ CentOS 등 레드햇 계열에서는 아무 옵션을 주지 않아도 홈 디렉토리 생성되고 쉘 환경이 설정됨

### 실행예시

```sh
[root@zetawiki ~]# useradd testuser
[root@zetawiki ~]# cat /etc/passwd | grep testuser
testuser:x:500:500::/home/testuser:/bin/bash
```

→ testuser 계정을 만들었다. UID와 GID는 500, 홈폴더는 /home/testuser 이고, bash 셸 사용이 가능하다.

```sh
[root@zetawiki~]# echo 'P@ssw0rd' | passwd --stdin testuser
Changing password for user testuser.
passwd: all authentication tokens updated successfully.
```

→ testuser 계정의 패스워드를 P@ssw0rd 로 설정하였다.

```sh
[root@zetawiki~]# passwd testuser
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```

→ testuser 계정의 패스워드를 설정.

<br/>

## 3. 그룹 지정하여 만들기

<br/>

명령어

```
useradd 계정명 -G 그룹명

-G 옵션: 그룹명이 없을 경우 사용(생성)
-g 옵션: 기존 그룹이 있을 경우 사용
```

<br/>

## 4. UID 지정하여 만들기

<br/>

사용자아이디(User ID; UID)는 숫자이다.

```
useradd 계정명 -u 사용자아이디
```

<br/>

## 추가

<br/>

### sudo 권한 부여.

```sh
[root@zetawiki~]# vi /etc/sudoers
```

```sh
## Next comes the main part: which users can run what software on
## which machines (the sudoers file can be shared between multiple
## systems).
## Syntax:
##
##      user    MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
maybe   ALL=(ALL)       ALL
jenkins ALL=(ALL)       ALL
svn     ALL=(ALL)       ALL
```

<br/>

### 계정삭제 : 계정 + 홈 디렉토리 삭제

```
userdel -r 계정명
```

→ 계정 삭제되고, 홈폴더도 삭제됨

<br/>

### 계정삭제 : 계정만 삭제

```
userdel 계정명
```

→ 계정은 삭제되고, 홈폴더는 남아있음
