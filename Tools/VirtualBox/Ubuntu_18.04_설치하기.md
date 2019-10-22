# Ubuntu 18.04 설치하기

<br/>

참고글 링크 - [VirtualBox 6.0을 사용하여 Ubuntu 18.04 설치하기](https://webnautes.tistory.com/448)

<br/>

**테스트 환경**

> OS : Windows 10 Pro 64 Bit <br/>
> CPU : Intel(R) Core(TM) i7-8550U CPU @ 1.80GHz <br/>
> Memory : 32GB <br/>
> Disk : 1TB

<br/>

**Ubuntu Download Web Site**

Download Ubuntu Desktop - Ubuntu 18.04.3 LTS

> https://ubuntu.com/download/desktop

Download Ubuntu Server - Ubuntu Server 18.04.3 LTS

> https://ubuntu.com/download/server

<br/>

**Ubuntu Desktop 18.04.3 LTS Recommended system requirements:**

- 2 GHz dual core processor or better
- 4 GB system memory
- 25 GB of free hard drive space
- Either a DVD drive or a USB port for the installer media
- Internet access is helpful

<br/>

Desktop 버전이아닌 Server 버전을 설치 할 경우 Desktop 버전의 자원 리소스의 반으로 설정을 하겠습니다.

설치되는 호스트 PC의 자원이 충분한 경우, 혹은 가상머신을 여러대 설치 하는 경우가 아니라면

Desktop 버전의 리소스 보다 약간 더 높에 설정하여 설치 합니다.

Server 버전인 경우에는 아래와 같이 설정 하도록 하겠습니다.

- 2 GHz dual core processor or better
- 2 GB system memory
- 20 GB of free hard drive space

<br/>

## 1. 새로운 가상머신 생성

<br/>

VirtualBox 관리자 상단에 위치한 `새로 만들기`를 클릭합니다.

새로운 가상 머신을 생성하기 위해 필요한 설정을 하기 위한 위자드가 실행됩니다.

![images](images/2019-10-18/2019-10-18_1508_17.png)

`이름`에 생성할 가상 머신의 이름을 적어주고 `종류`는 Linux, `버전`은 Ubuntu (64-bit)를 선택합니다.

다음을 클릭합니다.

![images](images/2019-10-18/2019-10-18_1508_18.png)

가상 머신에서 사용할 메모리를 할당해줍니다.

실제 메모리 크기의 50% 이내로 할당해야하며 초록색 범위를 넘으면 안됩니다.

![images](images/2019-10-18/2019-10-18_1508_19.png)

참고로 Ubuntu 18.04 다운로드 페이지에 나와있는 권장 메모리 크기는 2기가입니다.(Ubuntu Desktop Version)

가상 머신에서 사용할 가상 `하드 디스크`를 새로 생성하여 사용합니다.

`지금 새 가상 하드 디스크를 만들기`를 선택하고 `만들기 버튼`을 클릭합니다.

나중에 추가하거나 기존에 생성했던 가상 하드디스크를 가져와 사용할 수도 있습니다.

![images](images/2019-10-18/2019-10-18_1508_20.png)

`가상 하드 디스크 파일 종류`는 게스트 운영체제 성능에 크게 영향을 주지 않기 때문에 어떤 것을 선택하든 무방합니다.

대부분의 경우 `VDI (VirtualBox 디스크 이미지)`를 선택하면 됩니다.

다른 가상화 소프트웨어를 고려하여 파일 종류를 결정해도 되고 추후 포맷 변환을 해도 가능합니다.

VHD는 마이크로소프트의 가상 하드 디스크 기본 파일 포맷이며 VMDK는VMware의 가상 하드 디스크 기본 파일 포맷입니다.

![images](images/2019-10-18/2019-10-18_1508_21.png)

`물리적 하드 드라이브에 저장`하는 방식은 가상 머신의 성능을 고려하면 `고정 크기`를 권장합니다.

가상 머신 생성시 설정한 파일 크기로 파일을 미리 생성하기 때문에 동적 할당보다 약간 빠릅니다.

하지만 고정 크기로 하면 이 후 용량을 늘리려고 할때 번거롭기는 합니다.

(동적 할당으로 변환후 디스크 크기를 조정한 후, 다시 고정 크기로 변경해야 합니다. )

물리적 하드 디스크의 용량이 부족하거나 게스트 운영체제가 필요한 정확한 용량을 모르는 경우 `동적 할당`을 선택하는게 유리합니다.

처음에 작은 파일로 생성 되고 이후 데이터를 저장할 때 필요한 만큼 `설정한 파일 크기` 범위 내에서 가상 하드 디스크의 크기가 조정 됩니다.

이 방식을 사용하면 VirtualBox에서 제공하는 프로그램을 사용하여 쉽게 용량을 조정할 수 도 있습니다.

속도를 생각하면 `고정 크기`, 향후 디스크 크기 변동이 있을 듯하면 동적 할당을 선택하면 됩니다.

![images](images/2019-10-18/2019-10-18_1508_22.png)

`폴더 아이콘`을 클릭하여 가상 하드 디스크 파일이 저장될 위치를 지정합니다.

`파일 위치`를 변경하지 않으면 가상 머신은 `VirtualBox 관리자`에서 설정한 위치에 저장됩니다.

HDD보다 속도가 빠른 SSD에 저장해 놓고 사용하는 것을 권장합니다.

가상 하드 디스크로 사용되는 파일 크기를 설정합니다.

Ubuntu 18.04 다운로드 페이지에 나와있는 권장 하드 디스크 크기는 25 기가입니다. (Ubuntu Desktop Version)

여기서 정한 가상 하드 디스크의 용량이 부족시 가상 하드 디스크의 크기 조절을 할 수 있는 방법이 있긴하지만 시간이 많이드는 작업이라 신중하게 디스크 크기를 결정하세요.

우분투에서 권장하는 디스크 크기인 25기가보다 약간 크게 하면 될듯합니다. (Ubuntu Desktop Version)

![images](images/2019-10-18/2019-10-18_1508_23.png)

`만들기 버튼`을 클릭하면 가상 머신 생성이 시작됩니다.

물리적 하드 드라이브에 저장하는 방식을 `고정 크기`로 한 경우에는 시간이 오래 걸립니다.

<br/>

## 2. 가상 머신 설정

<br/>

새로 생성한 가상 머신의 설정을 일부 변경하기 해야 합니다. 해당 가상 머신이 선택된 상태에서 툴바에서 `설정`을 클릭합니다.

일부 옵션은 가상 머신이 `전원 꺼짐` 상태일 때만 설정이 가능합니다.

![images](images/2019-10-18/2019-10-18_1508_24.png)

`일반` 항목의 `고급` 탭에서는 `스냅샷 폴더(저장 위치)`, `클립보드 공유`, `드래그 앤 드롭` 설정을 할 수 있습니다.

![images](images/2019-10-18/2019-10-18_1508_25.png)

`스냅샷 폴더`에 가상머신의 상태인 스냅샷의 저장 위치는 디폴트로 가상 머신 저장 위치의 Snapshots 폴더로 되어 있습니다.

윈도우의 `시스템 복원` 기능과 유사하게 가상 머신의 상태를 스냅샷으로 저장해놓고 필요시 복원할 수 있습니다.

`클립보드 공유`는 게스트 운영체제와 호스트 운영체제 간에 클립보드 공유를 가능하게 해줍니다.

예를들어 게스트 운영체제에서 Ctrl + C를 눌러 클립 보드로 복사한 문자열을 호스트 운영체제에서 Ctrl + V를 눌러 원하는 곳에 붙여넣기 할 수 있습니다.

`게스트 운영체제에서 게스트 확장을 설치해야 사용가능합니다. 나중에 설명합니다.`

`양방향`으로 설정합니다.

`드래그 앤 드롭`은 파일을 드래그하여 게스트 운영체제와 호스트 운영체제 간에 파일 복사를 가능하도록 해줍니다.

일부 게스트 운영체제에서는 제대로 동작안 할 수 있습니다.

`게스트 운영체제에서 게스트 확장을 설치해야 사용가능합니다. 나중에 설명합니다.`

`양방향`으로 설정합니다.

`시스템` 항목의 `마더보드` 탭에서는 가상 머신에서 사용할 `메모리 크기`, `부팅 순서`, `마더보드 칩셋 종류`, `포인팅 장치` 등을 설정할 수 있습니다.

![images](images/2019-10-18/2019-10-18_1508_26.png)

가상 머신에서 사용할 메모리(RAM)를 `기본 메모리`에서 조정할 수 있습니다.

사용자가 지정한 메모리는 호스트 운영체제에서 제공됩니다.

`부팅 순서`에서는 가상 머신에서 사용하는 미디어들의 부팅 순서를 지정합니다.

사용하지 않는 플로피 디스크는 체크 해제합니다.

메인보드의 `칩셋`으로 PIIX3와 ICH9 중 하나를 선택할 수 있습니다. `ICH9`로 변경합니다.

ICH9는 실험적인 기능이어서 필수적으로 요구하는 맥OS나 최근에 나온 운영체제에서만 사용해야만 합니다.

`포인팅 장치`는 마우스 동작과 관련 있습니다. 구형 운영체제를 게스트 운영체제로 사용한다면 PS/2로 선택해야 할 수 있습니다.

<br/>

### I/O APIC 사용하기

게스트 운영체제로 윈도우 또는 64비트 운영체제를 사용하는 경우에는 체크해야 합니다.

가상 머신에서 2개 이상의 CPU 코어를 사용할 경우에도 체크해야 합니다. 체크하면 가상 머신의 성능이 약간 저하 될 수 있습니다.

UEFI 모드 부팅 가능하도록 설치 미디어가 준비된 경우에만 `EFI 사용하기`를 체크해야 합니다.

Ubuntu 18.04 ISO 이미지로도 UEFI 부팅 가능하니 체크합니다.

<br/>

### 하드웨어 시각을 UTC로 보고하기

호스트 운영체제의 로컬 시간 대신 게스트 운영체제에 UTC 형식의 시스템 시간을 제공합니다.

유닉스/리눅스를 게스트 운영체제로 사용할 경우 체크합니다.

<br/>

`시스템` 항목의 `프로세스` 탭에서는 `가상머신에서 사용할 프로세스 개수`, `실행 제한`, `PAE/NX 사용 여부`를 설정 할 수 있습니다.

![images](images/2019-10-18/2019-10-18_1508_27.png)

<br/>

### 프로세서 개수

게스트 운영체제에서 사용하게 될 가상 CPU 코어 개수를 설정합니다. 실제 호스트 컴퓨터의 코어 개수의 절반 이하로 설정해야 합니다.

VirtualBox 문서에는 실제 코어 개수 보다 많이 할당하지 말라고 나옵니다. 논리 프로세서가 아닌 코어를 기준으로 할당하라는 말 같습니다.

```
You should not, however, configure virtual machines to use more CPU cores than you have available physically (real cores, no hyperthreads).


https://www.virtualbox.org/manual/ch03.html#settings-processor
```

<br/>

### 실행 제한

가상 프로세서를 에뮬레이터하기 위해 호스트 컴퓨터의 프로세서가 소비하는 시간을 제한합니다.

100%로 설정하면 가상 프로세서로 할당된 호스트 컴퓨터의 프로세서는 에뮬레이터를 위해서만 사용됩니다.

실행 제한 비율을 너무 낮게 잡으면 가상 머신이 느리게 동작할 수 있습니다.

<br/>

### PAE/NX 사용하기

가상머신에 설치된 운영체제가 4기가 이상의 메모리를 사용하기 위해서 체크해줘야 합니다.

<br/>

시스템 항목의 가속 탭에서는 VirtualBox에서 하드웨어 가상화를 사용하도록 설정할 수 있습니다.

![images](images/2019-10-18/2019-10-18_1508_28.png)

<br/>

### VT-x/AMD-V 사용하기

호스트 CPU에서 제공하는 가상화 확장을 사용할지 여부를 선택합니다.

사용하려면 바이오스에서 활성화되어 있어야 하고 64비트 게스트 운영체제를 사용하려면 활성화해야합니다.

<br/>

### 네스티드 페이징 사용하기

가상 머신에서 VT-x/AMD-V의 네스티드 페이징(Nested paging)을 사용여부를 선택합니다.

하드웨어 가상화를 활성화 했다면 설정해줘야 합니다.

설정하면 하드웨어에서 메모리 관리를 하기 때문에 가상화 소프트웨어에서 이 작업을 수행 할 필요가 없습니다.

따라서 하드웨어 가상화의 성능 향상이 됩니다.

`VT-x/AMD-V 사용하기`와 `네스티드 페이징 사용하기`를 같이 사용하면 가상 머신의 성능 향상이 됩니다.

<br/>

디스플레이 항목의 화면 탭에서는 비디오 메모리, 모니터 개수, 크기 조정 비율, 가속 사용여부를 설정할 수 있습니다.

![images](images/2019-10-18/2019-10-18_1508_29.png)

<br/>

### 비디오 메모리

가상 머신의 그래픽 기능을 위해 사용할 비디오 메모리 크기입니다.

높은 해상도 전체 화면으로 게스트 운영체제를 보려면 충분한 크기를 할당해야 합니다.

호스트 운영체제에서 물리적인 메모리(RAM)로부터 할당해줍니다.

<br/>

### 모니터 개수

하나 이상의 가상 모니터를 가상 머신에 제공할 수 있습니다.

전체 화면 또는 심리스 모드의 경우에는 물리적인 모니터가 사용할 개수 만큼 있어야 합니다.

<br/>

### 3차원 가속 사용하기

호스트 컴퓨터에서 3차원 그래픽 가속이 가능한 경우 체크하면 가상 머신에서도 3차원 그래픽 가속을 사용할 수 있습니다.

<br/>

### 2차원 비디오 가속 사용하기

체크하면 가상 머신에서 호스트 컴퓨터의 비디오 가속 기능을 사용할 수 있습니다.

게스트 운영체제가 윈도우인 경우에만 가능한 옵션입니다.

<br/>

이제 가상 머신에 설치할 설치용 ISO 이미지를 선택해줄 차례입니다.

`저장소 항목`에서 `컨트롤러:IDE` 항목에 보이는 `광학 디스크 아이콘`을 클릭합니다.

![images](images/2019-10-18/2019-10-18_1508_30.png)

`디스크 선택하기`를 클릭하고 다운로드 받아놓았던 우분투 ISO 이미지를 선택해줍니다.

![images](images/2019-10-18/2019-10-18_1508_31.png)

다음처럼 IDE 컨트롤러에 ISO 이미지가 삽입됩니다.

![images](images/2019-10-18/2019-10-18_1508_32.png)

오디오 항목에서는 오디오 드라이버와 컨트롤러를 변경할 수 있습니다.

가상 머신에서 마이크를 사용해야 한다면 `오디오 입력 사용하기`를 체크하세요.

![images](images/2019-10-18/2019-10-18_1508_33.png)

`네트워크` 항목에서 `네트워크 어댑터와 네트워크 연결 방식`을 구성 할 수 있습니다.

NatNetwork 를 사용하기 위해서는 `VirtualBox > 파일 > 환경설정 > 네트워크` 탭에서 NAT 네트워크를 추가 해줘야 합니다.

![images](images/2019-10-18/2019-10-18_1508_34.png)

Host PC 내의 가상 머신 간의 통신 가능한 `NAT 네트워크`를 사용하도록 하겠습니다.

![images](images/2019-10-18/2019-10-18_1508_35.png)

Host PC 에서 SSH 접속을 위해 `호스트 전용 어탭터`도 설정을 합니다.

![images](images/2019-10-18/2019-10-18_1508_36.png)

설정이 완료 되었다면 `시작` 버튼을 클릭 하여 Ubuntu 설치를 진행 합니다.

<br/>

## 3. Ubuntu 설치.

<br/>

Please choose your preferred language > English

![images](images/2019-10-21/2019-10-21_1808_01.png)

Keyboard configuration

- Layout : Korean
- Variant : Korean - Korean (101/104 key compatible)

![images](images/2019-10-21/2019-10-21_1808_02.png)

Network connections

- PASS/NONE : Ubuntu 설치 완료 후 수정

![images](images/2019-10-21/2019-10-21_1808_03.png)

Configure proxy

- PASS/NONE

![images](images/2019-10-21/2019-10-21_1808_04.png)

Configure Ubuntu archive mirror

- PASS/NONE : Ubuntu 설치 완료 후 수정

![images](images/2019-10-21/2019-10-21_1808_05.png)

Filesystem setup

- Use An Entire Disk (권장사항)

![images](images/2019-10-21/2019-10-21_1808_06.png)

![images](images/2019-10-21/2019-10-21_1808_07.png)

![images](images/2019-10-21/2019-10-21_1808_08.png)

Profile setup

- Your name: maybe
- Your server's name: ubuntu
- Pick a username: maybe
- Choose a password: ???????
- Confirm your password: ???????

![images](images/2019-10-21/2019-10-21_1808_09.png)

SSH Setup

- PASS/NONE : Ubuntu 설치 완료 후 설치

![images](images/2019-10-21/2019-10-21_1808_10.png)

Featured Server Snaps

- PASS/NONE

![images](images/2019-10-21/2019-10-21_1808_11.png)

설치중...

![images](images/2019-10-21/2019-10-21_1808_12.png)

설치완료... Reboot

![images](images/2019-10-21/2019-10-21_1808_13.png)

설치 후 로그인

![images](images/2019-10-21/2019-10-21_1808_14.png)

<br/>

## 4. Ubuntu 설정

<br/>

### Root 권한 생성 / Root 계정 활성화.

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

<br/>

### ssh 서버 설치.

```sh
### 1. 설치확인
root@ubuntu:~# netstat -ntlp | grep sshd
root@ubuntu:~# service ssh status
Unit ssh.service could not be found.
### → 설치 안됨

root@ubuntu:~# dpkg --get-selections | grep ssh
openssh-client					install
### → openssh-client만 설치되어 있다. openssh-server가 필요하다.

### 2. 설치 및 시작
root@ubuntu:~# apt-get install openssh-server
...(생략)
root@zetawiki:~# service ssh restart

### 3. 확인
root@ubuntu:~# dpkg --get-selections | grep ssh
openssh-client					install
openssh-server					install
openssh-sftp-server     install
ssh-import-id					  install
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# service ssh status
ssh start/running, process 1702
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# ps -ef | grep sshd | grep -v grep
root      1702     1  0 03:53 ?        00:00:00 /usr/sbin/sshd -D
root@ubuntu:~#
root@ubuntu:~#
root@ubuntu:~# netstat -ntlp | grep sshd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1702/sshd
tcp6       0      0 :::22                   :::*                    LISTEN      1702/sshd

### 4. ssh root 계정 접속 허용 설정
root@ubuntu:~# vi /etc/ssh/sshd_config
### PermitRootLogin prohibit_password 주석 해제 후
### PermitRootLogin yes 로 수정.
### ssh restart
root@ubuntu:~# service ssh restart
```

<br/>

### 네트워크 고정 IP 설정

Ubuntu 18 LTS 부터는 Netplan 이 적용되어 설정 방식이 변경됨.

netplan 은 yaml 을 사용.

고정 IP 설정시 `dhcp4: no` 를 꼭 추가

```sh
### 1. 현재 gateway 확인
### route
### netstat -r
root@ubuntu:~# route
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
default         _gateway        0.0.0.0         UG    100    0        0 enp0s3
10.0.2.0        0.0.0.0         255.255.255.0   U     0      0        0 enp0s3
_gateway        0.0.0.0         255.255.255.255 UH    100    0        0 enp0s3
192.168.56.0    0.0.0.0         255.255.255.0   U     0      0        0 enp0s8
root@ubuntu:~#

root@ubuntu:~# netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         _gateway        0.0.0.0         UG        0 0          0 enp0s3
10.0.2.0        0.0.0.0         255.255.255.0   U         0 0          0 enp0s3
_gateway        0.0.0.0         255.255.255.255 UH        0 0          0 enp0s3
192.168.56.0    0.0.0.0         255.255.255.0   U         0 0          0 enp0s8
root@ubuntu:~#

### 2. 현재 네트워크 설정 확인.
root@ubuntu:~# vi /etc/netplan/50-cloud-init.yaml

### 3. 네트웍 설정 디렉토리 이동.
root@ubuntu:~# cd /etc/netplan

### 4. 현재 설정 파일 복사
root@ubuntu:/etc/netplan# cp -arp 50-cloud-init.yaml 01-netcfg.yaml

### 5. 복사한 설정파일 편집
root@ubuntu:/etc/netplan# vi /etc/netplan/01-netcfg.yaml

# This file is generated from information provided by
# the datasource.  Changes to it will not persist across an instance.
# To disable cloud-init's network configuration capabilities, write a file
# /etc/cloud/cloud.cfg.d/99-disable-network-config.cfg with the following:
# network: {config: disabled}
network:
    ethernets:
        enp0s3:
            dhcp4: no
            addresses: [10.0.2.100/24]
            gateway4: 10.0.2.1
        enp0s8:
            dhcp4: no
            addresses: [192.168.56.100/24]
            gateway4: 192.168.56.0
            nameservers
                addresses: [8.8.8.8,8.8.4.4]
    version: 2

### enp0s3 ethernets 는 Host PC 내의 가상 머신 간의 통신이 가능한 IP 이다.
### enp0s8 ethernets 는 같은 Host PC 내의 가상 머신 간 연결이 가능한 IP 이다.

### 6. 네트워크 변경사항 반영
root@ubuntu:/etc/netplan# netplan apply

### 7. 변경사항 확인
root@ubuntu:/etc/netplan# ip addr
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host
       valid_lft forever preferred_lft forever
2: enp0s3: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:83:4a:21 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.100/24 brd 10.0.2.255 scope global enp0s3
       valid_lft forever preferred_lft forever
    inet 10.0.2.8/24 brd 10.0.2.255 scope global secondary dynamic enp0s3
       valid_lft 1149sec preferred_lft 1149sec
    inet6 fe80::a00:27ff:fe83:4a21/64 scope link
       valid_lft forever preferred_lft forever
3: enp0s8: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 08:00:27:d3:9e:7a brd ff:ff:ff:ff:ff:ff
    inet 192.168.56.100/24 brd 192.168.56.255 scope global enp0s8
       valid_lft forever preferred_lft forever
    inet 192.168.56.105/24 brd 192.168.56.255 scope global secondary dynamic enp0s8
       valid_lft 1149sec preferred_lft 1149sec
    inet6 fe80::a00:27ff:fed3:9e7a/64 scope link
       valid_lft forever preferred_lft forever
root@ubuntu:/etc/netplan#
root@ubuntu:/etc/netplan#
root@ubuntu:/etc/netplan# ip route
default via 10.0.2.1 dev enp0s3 proto static
default via 10.0.2.1 dev enp0s3 proto dhcp src 10.0.2.8 metric 100
10.0.2.0/24 dev enp0s3 proto kernel scope link src 10.0.2.100
10.0.2.1 dev enp0s3 proto dhcp scope link src 10.0.2.8 metric 100
192.168.56.0/24 dev enp0s8 proto kernel scope link src 192.168.56.100
root@ubuntu:/etc/netplan#

### 8. 구글 DNS 가 정상적으로 나오는지 확인
root@ubuntu:/etc/netplan# nslookup google.com
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   google.com
Address: 172.217.26.46
Name:   google.com
Address: 2404:6800:4004:80e::200e

root@ubuntu:/etc/netplan#

### 9. Secure Sell 을 이용한 접속확인.
### Putty, MobaXterm 등을 이용하여 설정한 고정IP로 접속을 시도해본다.
### ex. IP : 192.168.56.0 / Port : 22
```

<br/>
