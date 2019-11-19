# Root 권한 생성 / Root 계정 활성화.

<br/>

우분투(Ubuntu)는 보안상의 이유로 root 계정으로 로그인하는 것을 막아두었습니다.

만약 root 계정으로 접속하여 관리하고 싶다면 추가적인 작업이 필요합니다.

설치할 때 만든 사용자 계정으로 로그인한 후 다음과 같이 명령하여 root 계정의 비밀번호를 생성합니다.

```sh
### 현재 계정의 패스워드 입력.
maybe@ubuntu:~$ sudo passwd root
[sudo] password for maybe:

### 사용할 root 패스워드 2번 입력.
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully

### 확인.
maybe@ubuntu:~$ su
Password:
root@ubuntu:/home/maybe# whoami
root
```
