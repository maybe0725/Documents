# Ubuntu 18.04 자동실행, 서비스 등록

<br/>

출처 :: [Ubuntu 18.04 자동실행, 서비스 등록](https://nasanx2001.tistory.com/entry/%EC%9A%B0%EB%B6%84%ED%88%AC-1804-%EC%9E%90%EB%8F%99%EC%8B%A4%ED%96%89-%EC%84%9C%EB%B9%84%EC%8A%A4%EB%93%B1%EB%A1%9D)

<br/>

기존에 사용했던 rc.local 은 18.04 LTS 버전으로 업그레이드 되면서 요즘 추세대로 systemd 형식을 따라가는거 같다.

그래서 rc.local 은 사라져 사용할 수가 없다.

물론 따로 활성화 시켜서 사용해도 되지만 결국 서비스를 올려서 사용해야 하는 부분은 여전이 동일함으로,

차라리 원하는 프로그램을 서비스로 등록해서 사용하는 것이 좀더 효율적 이라고 판단.

아래와 같이 서비스를 등록하고 사용하면 된다.

<br/>

## 1. Service 파일 만들기

<br/>

root 권한으로 `/etc/systemd/system/` 위치에 원하는 `service file`을 만든다.

service의 이름이 `svnserve` 라면 service file이름은 `svnserve.service`으로 한다.

```sh
sudo vi /etc/systemd/system/svnserve.service
```

<br/>

## 2. Service 파일 내용 작성

```sh
[Unit]
### 서비스 제목 또는 설명
Description=Subversion

[Service]
### 실행파일 경로
ExecStart=/app/svn/bin/svnserve -d --threads -r /app/svn
### 실행파일 경로
ExecStop=ps -ef | grep svnserve | grep -v grep | awk '{print "kill -9", $2}' | sh
### restart 조건 (on-failure : 오류 발생시 재시작)
Restart=on-failure
### restart 시간
RestartPreventExitStatus=255

[Install]
### "systemctl enable" 명령어로 유닛을 등록할때 등록에 필요한 유닛을 지정한다. (종속성 검사 단계)
WantedBy=multi-user.target
Alias=sshd.service
```

<br/>

### 3. Service 등록 및 실행을 위한 권한부여

```sh
chmod +x svnserve.service
or
chmod 755 svnserve.service

systemctl daemon-reload

systemctl enable svnserve.service

systemctl start svnserve.service

systemctl status svnserve.service
```

<br/>

# 검증실패

<br/>

서비스 상태가 'dead' 로 나타남.

원인 및 해결책을 모르겠음.

```sh
root@ubuntu-deploy:/etc/systemd/system# systemctl status svnserve.service
● svnserve.service - Subversion
   Loaded: loaded (/etc/systemd/system/svnserve.service; enabled; vendor preset: enabled)
   Active: inactive (dead) since Tue 2019-10-29 05:35:37 UTC; 27min ago
  Process: 1964 ExecStart=/app/svn/bin/svnserve -d --threads -r /app/svn (code=exited, status=0/SUCCESS)
 Main PID: 1964 (code=exited, status=0/SUCCESS)

Oct 29 05:35:37 ubuntu-deploy systemd[1]: Started Subversion.
root@ubuntu-deploy:/etc/systemd/system#
```

https://nasanx2001.tistory.com/entry/%EC%9A%B0%EB%B6%84%ED%88%AC-1804-%EC%9E%90%EB%8F%99%EC%8B%A4%ED%96%89-%EC%84%9C%EB%B9%84%EC%8A%A4%EB%93%B1%EB%A1%9D

https://khann.tistory.com/5
