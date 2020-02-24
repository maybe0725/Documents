# Ubuntu Docker 설치

<br/>

> 출처 - https://kururu.tistory.com/87

> 참고 - https://docs.docker.com/install/linux/docker-ce/ubuntu/ <br/>
> 참고 - https://zetawiki.com/wiki/%EC%9A%B0%EB%B6%84%ED%88%AC16_docker_CE_%EC%84%A4%EC%B9%98

<br/>

- Community Edition (CE) : 개발자나 작은 팀들에게 이상적인 버전이며 무료 사용 가능.

- Enterprise Edition (EE) : 엔터프라이즈 개발이나 실제 확장 가능한 서비스를 개발하는 팀에서 사용하기 적합한 유료버전.

<br/>

## Docker 설치 확인 여부

```sh
$ docker -v

Command 'docker' not found, but can be installed with:

sudo snap install docker     # version 18.09.9, or
sudo apt  install docker.io

See 'snap info docker' for additional versions.
```

## 설치 리스트 확인

```sh
$ apt list docker docker-engine docker.io

Listing... Done
docker/bionic 1.5-1build1 amd64
docker.io/bionic-updates,bionic-security 18.09.7-0ubuntu1~18.04.4 amd64
```

## 업데이트

```sh
$ sudo apt update

[sudo] password for maybe:

Hit:1 http://kr.archive.ubuntu.com/ubuntu bionic InRelease
Hit:2 http://kr.archive.ubuntu.com/ubuntu bionic-updates InRelease
Hit:3 http://kr.archive.ubuntu.com/ubuntu bionic-backports InRelease
Hit:4 http://kr.archive.ubuntu.com/ubuntu bionic-security InRelease
Reading package lists... Done
Building dependency tree
Reading state information... Done
65 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

## 설치

### apt install

```sh
$ sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

### apt-key

```sh
### 아래 두 가지 방법 중 하나 실행

### Case 1.
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

### Case 2.
$ curl -kv -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```sh
$ apt-key fingerprint 0EBFCD88
pub   rsa4096 2017-02-22 [SCEA]
      9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid           [ unknown] Docker Release (CE deb) <docker@docker.com>
sub   rsa4096 2017-02-22 [S]
```

### add-apt-repository

```sh
$ sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
Get:1 https://download.docker.com/linux/ubuntu bionic InRelease [64.4 kB]
Get:2 https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages [10.3 kB]
Hit:3 http://kr.archive.ubuntu.com/ubuntu bionic InRelease
Get:4 http://kr.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:5 http://kr.archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Get:6 http://kr.archive.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Fetched 327 kB in 3s (95.0 kB/s)
Reading package lists... Done
```

stable는 안정판인데 목적에 따라 nightly, test 를 사용할 수 있다.

`$(lsb_release -cs)` 는 현재 명령이 실행되는 우분투 배포판 이름을 가져온다.

필요하다면 직접 넣을 수 있다. ex) precise 등.

```sh
$ cat /etc/apt/sources.list | grep docker
deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
# deb-src [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable
```

### apt update

```sh
$ sudo apt update
Hit:1 https://download.docker.com/linux/ubuntu bionic InRelease
Hit:2 http://kr.archive.ubuntu.com/ubuntu bionic InRelease
Get:3 http://kr.archive.ubuntu.com/ubuntu bionic-updates InRelease [88.7 kB]
Get:4 http://kr.archive.ubuntu.com/ubuntu bionic-backports InRelease [74.6 kB]
Get:5 http://kr.archive.ubuntu.com/ubuntu bionic-security InRelease [88.7 kB]
Fetched 252 kB in 4s (66.9 kB/s)
Reading package lists... Done
Building dependency tree
Reading state information... Done
63 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

### apt install

### 도커 설치

```sh
$ sudo apt install docker-ce -y
```

### 도커 설치 - 특정 버전

```sh
$ apt-cache madison docker-ce
 docker-ce | 5:19.03.6~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.5~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.4~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.3~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.2~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.1~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:19.03.0~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.9~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.8~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.7~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.6~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.5~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.4~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.3~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.2~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.1~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 5:18.09.0~3-0~ubuntu-bionic | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.3~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.2~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.06.0~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages
 docker-ce | 18.03.1~ce~3-0~ubuntu | https://download.docker.com/linux/ubuntu bionic/stable amd64 Packages

# 위 버전 중 원하는 버전을 입력하여 설치한다.
$ sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io

# ex) sudo apt-get install docker-ce=18.06.1~ce~3-0~ubuntu docker-ce-cli=18.06.1~ce~3-0~ubuntu containerd.io
```

### 도커 권한 확인

```sh
$ cat /etc/group | grep docker
docker:x:999:
```

### 도커 그룹에 사용할 사용자 아이디 추가

```sh
$ sudo usermod -aG docker 사용할아이디

ex) $ sudo usermod -aG docker iamdeveloper
```

### 도커 권한 확인

아래와 같이 사용자 아이디 추가됨

```sh
$ cat /etc/group | grep docker
docker:x:999:maybe
```

시스템을 재 시작하여 사용자 권한으로 도커 사용 가능한지 확인

```sh
sudo reboot
```

### 사용자 계정으로 도커 사용 확인

```sh
$ docker version
Client: Docker Engine - Community
 Version:           19.03.6
 API version:       1.40
 Go version:        go1.12.16
 Git commit:        369ce74a3c
 Built:             Thu Feb 13 01:27:49 2020
 OS/Arch:           linux/amd64
 Experimental:      false

Server: Docker Engine - Community
 Engine:
  Version:          19.03.6
  API version:      1.40 (minimum version 1.12)
  Go version:       go1.12.16
  Git commit:       369ce74a3c
  Built:            Thu Feb 13 01:26:21 2020
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.2.10
  GitCommit:        b34a5c8af56e510852c35414db4c1f4fa6172339
 runc:
  Version:          1.0.0-rc8+dev
  GitCommit:        3e425f80a8c931f88e6d94a8c831b9d5aa481657
 docker-init:
  Version:          0.18.0
  GitCommit:        fec3683
```

`Client:` 와 `Server:` 정보가 나오면 사용 가능함.

아래 메시지가 나오면 권한 설정이 제대로 된 것이 아니므로 다시 설정시도하거나 관련 메시지로 구글링하여 해결한다.

```sh
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock: Get http://%2Fvar%2Frun%2Fdocker.sock/v1.40/images/json: dial unix /var/run/docker.sock: connect: permission denied
```

## 버전확인

```sh
$ apt list docker-ce -a
Listing... Done
docker-ce/bionic,now 5:19.03.6~3-0~ubuntu-bionic amd64 [installed]
docker-ce/bionic 5:19.03.5~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:19.03.4~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:19.03.3~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:19.03.2~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:19.03.1~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:19.03.0~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.9~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.8~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.7~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.6~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.5~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.4~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.3~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.2~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.1~3-0~ubuntu-bionic amd64
docker-ce/bionic 5:18.09.0~3-0~ubuntu-bionic amd64
docker-ce/bionic 18.06.3~ce~3-0~ubuntu amd64
docker-ce/bionic 18.06.2~ce~3-0~ubuntu amd64
docker-ce/bionic 18.06.1~ce~3-0~ubuntu amd64
docker-ce/bionic 18.06.0~ce~3-0~ubuntu amd64
docker-ce/bionic 18.03.1~ce~3-0~ubuntu amd64

$ docker -v
Docker version 19.03.6, build 369ce74a3c
```

## 테스트

```sh
$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
1b930d010525: Pull complete
Digest: sha256:fc6a51919cfeb2e6763f62b6d9e8815acbf7cd2e476ea353743570610737b752
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker ID:
 https://hub.docker.com/

For more examples and ideas, visit:
 https://docs.docker.com/get-started/
```

## 도커 서비스 실행여부확인

```sh
$ sudo systemctl status docker
● docker.service - Docker Application Container Engine
   Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
   Active: active (running) since Mon 2020-02-24 01:23:07 UTC; 16min ago
     Docs: https://docs.docker.com
 Main PID: 4579 (dockerd)
    Tasks: 11
   CGroup: /system.slice/docker.service
           └─4579 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock

Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.277676929Z" level=warning msg="Your kernel does not support cgroup rt period"
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.277734883Z" level=warning msg="Your kernel does not support cgroup rt runtime"
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.277925992Z" level=info msg="Loading containers: start."
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.564355069Z" level=info msg="Default bridge (docker0) is assigned with an IP address 172.17.0.0/16. Daemon option --bip can be used to set a preferred IP address"
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.776642362Z" level=info msg="Loading containers: done."
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.831404021Z" level=info msg="Docker daemon" commit=369ce74a3c graphdriver(s)=overlay2 version=19.03.6
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.831707435Z" level=info msg="Daemon has completed initialization"
Feb 24 01:23:07 docker-demo dockerd[4579]: time="2020-02-24T01:23:07.876214367Z" level=info msg="API listen on /var/run/docker.sock"
Feb 24 01:23:07 docker-demo systemd[1]: Started Docker Application Container Engine.
Feb 24 01:25:46 docker-demo dockerd[4579]: time="2020-02-24T01:25:46.192988531Z" level=info msg="ignoring event" module=libcontainerd namespace=moby topic=/tasks/delete type="*events.TaskDelete"
```

## 도커 삭제

### 1. Uninstall the Docker Engine - Community package

```sh
$ sudo apt-get purge docker-ce
```

### 2. Images, containers, volumes, or customized configuration files on your host are not automatically removed. To delete all images, containers, and volumes:

```sh
$ sudo rm -rf /var/lib/docker
```
