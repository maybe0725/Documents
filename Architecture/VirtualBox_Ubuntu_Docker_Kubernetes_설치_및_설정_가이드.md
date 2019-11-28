# VirtualBox Ubuntu Kubernetes 설치 및 설정 가이드

<br/>

## 1. 사전준비

<br/>

- Ubuntu 18.04.3 LTS Server Download (http://releases.ubuntu.com/18.04/)

- VirtualBox Download (https://www.virtualbox.org/wiki/Downloads)

- MobaXterm Download (https://mobaxterm.mobatek.net/download.html)

<br/>

## 2. 가상 머신 Nodes Network 구성

<br/>

```
Host PC  ───  NAT Network (가상공유기)  ─┬─  < Docker 실습용 >
                                        │     hostname : vmdemo
                                        │     enp0s3 ip : 10.0.2.220
                                        │     enp0s8 ip : 192.168.56.220
                                        │
                                        │
                                        ├─  < Kubernetes Master Node >
                                        │     hostname : kubemaster
                                        │     enp0s3 ip : 10.0.2.200
                                        │     enp0s8 ip : 192.168.56.200
                                        │
                                        │
                                        ├─  < Kubernetes Worker Node 1 >
                                        │     hostname : kubenode01
                                        │     enp0s3 ip : 10.0.2.201
                                        │     enp0s8 ip : 192.168.56.201
                                        │
                                        │
                                        ├─  < Kubernetes Worker Node 2 >
                                        │     hostname : kubenode02
                                        │     enp0s3 ip : 10.0.2.202
                                        │     enp0s8 ip : 192.168.56.202
                                        │
                                        │
                                        └─  < Kubernetes Worker Node 3 >
                                              hostname : kubenode03
                                              enp0s3 ip : 10.0.2.203
                                              enp0s8 ip : 192.168.56.203
```

<br/>

## 3. 가상 머신 만들기

<br/>

### 3.1. NATNetwork 추가

※ Kubernetes Node 간 통신할 수 있도록 “NATNetwotk” 추가.

VirtualBox > 파일 > 환경설정(Ctrl+G) > 네트워크> NAT 네트워크 > 새 NAT 네트워크 추가(Icon)

<br/>

### 3.2. 가상 머신 만들기

- VirtualBox > 머신 > 새로만들기(Ctrl+N)

- STEP 01. 이름 및 운영 체제 <br/>
  이름 : KubeMaster <br/>
  머신폴더 : C:\Users\admin\VirtualBox VMs <br/>
  종류 : Linux <br/>
  버전 : Ubuntu (64-bit) <br/>

- STEP 02. 메모리 크기 <br/>
  2048 MB ~ 4096 MB

- STEP 03. 하드 디스크 <br/>
  ‘지금 새 가상 하드 디스크 만들기’ 선택

- STEP 04. 하드 디스크 파일 종류 <br/>
  VDI(VirtualBox 디스크 이미지)

- STEP 05. 물리적 하드 드라이브에 저장 <br/>
  동적 할당

- STEP 06. 파일 위치 및 크기 <br/>
  파일위치 : C:\Users\admin\VirtualBox VMs\이하 디렉토리 <br/>
  크기 : 10.00 GB

<br/>

## 4. 가상 머신 설정

<br/>

### 4.1. 시스템

※ Kubernetes 의 모든 Node 는 프로세서 개수를 2 개 이상으로 설정을 해줘야 Kubernetes 가 정상적으로 설치된다.

가상 머신 선택 > 설정(S) > 시스템 > 프로세서 TAB > 프로세서 개수 ‘2’

<br/>

### 4.2. 네트워크

※ Kubernetes 의 모든 Node 간 통신이 가능하도록 가상머신에 “NAT 네트워크”를 설정해줍니다. (주의!“NAT”로 설정하면 안됩니다.)

가상 머신 선택 > 설정(S) > 네트워크 > 어댑터 1 TAB <br/>

- 네트워크 어댑터 사용하기 : [Check] <br/>
- 다음에 연결됨 : NAT 네트워크 <br/>
- 이름 : NatNetwork <br/>
- 어댑터 종류 : Intel PRO/1000 MT Desktop (82540EM) <br/>
- 무작위 모드 : 거부 <br/>
- MAC 주소 : 080027100D2E (가상 머신 마다 다를 수 있음) <br/>
- 케이블 연결됨 : [Check]

<br/>

※ 외부에서 Kubernetes 의 모든 Node 간 통신할 수 있도록 “호스트 전용 어댑터”를 설정해줍니다

가상 머신 선택 > 설정(S) > 네트워크 > 어댑터 2 TAB <br/>

- 네트워크 어댑터 사용하기 : [Check] <br/>
- 다음에 연결됨 : 호스트 전용 어댑터 <br/>
- 이름 : VirtualBox Host-Only Ethernet Adapter <br/>
- 어댑터 종류 : Intel PRO/1000 MT Desktop (82540EM) <br/>
- 무작위 모드 : 거부 <br/>
- MAC 주소 : 08002717EA34 (가상 머신 마다 다를 수 있음) <br/>
- 케이블 연결됨 : [Check]

<br/>

## 5. Ubuntu 설치

<br/>

### 5.1. Ubuntu iso File Mount

※ 가상 머신 선택 > 설정(S) > 저장소 > 저장 장치 > 컨트롤러 : IDE > ‘비어 있음’ 드라이브 선택

※ 속성 > 광학 드라이브 > 드라이브 아이콘(Icon) 클릭 > 다운받은 Linux Ubuntu iso 파일 선택

※ 가상 머신 ‘시작‘

<br/>

### 5.2. Ubuntu 설치

STEP 01. choose language : English

STEP 02. keyboard configuration

- Layout : Korean <br/>
- Variant : Korean – Korean (101/104 Key compatible)

STEP 03. Network connections

- Skip : 설치 후 따로 설정

STEP 04. Configure proxy

- Skip

STEP 05. Configure Ubuntu archive mirror

- Default

- [지역별 우분투 미러 서버 리스트](https://launchpad.net/ubuntu/+cdmirrors)

- 지역별 저장소 변경 방법(archive mirror)

```sh
$ vi /etc/apt/sources.list

# http://archive.ubuntu.com/ubuntu 를 모두 변경합니다.
# 만약 한국이라면 http://mirror.kakao.com/ubuntu 으로 변경.
```

STEP 06. Filesystem setup

- Default

STEP 07. Profile setup

- Your name : kube
- Your server’s name : kubemaster
- Pick a username : kube
- Choose a password : ???????
- Confirm your password : ???????

STEP 08. SSH Setup

- Skip : Ubuntu 설치 후 설치

STEP 09. Featured Server Snaps

- etcd [Check]
- Kubernetes 의 Master Node가 사용하게 될 ConfigDatabase인 etcd 를 설치.

STEP 10. 설치 후 Reboot

<br/>

## 6. Ubuntu 환경 설정

<br/>

### 6.1. Cloud-init Service delete

Ubuntu 설치 후 Cloud-init 이라는 서비스가 시작되는데 필요 없으므로 제거를 한다.

```sh
$ echo 'datasource_list: [ None ]' | sudo -s tee /etc/cloud/cloud.cfg.d/90_dpkg.cfg
#$ sudo apt-get purge cloud-init
$ sudo apt purge cloud-init
$ sudo rm –rf /etc/cloud/; sudo rm –rf /var/lib/cloud/
$ reboot
```

Linux cloud-initdisabled 확인:

```sh
$ sudo touch /etc/cloud/cloud-init.disabled
```

<br/>

### 6.2 openssh-server 설치

```sh
$ dpkg -l | grep openssh-server
#$ sudo apt-get install openssh-server
$ sudo apt install openssh-server
$ sudo service ssh start
```

<br/>

### 6.3. root 계정 접속 허용 설정 # ssh 설정파일 편집

```sh
$ sudo vi /etc/ssh/sshd_config
# PermitRootLogin 속성값을 ‘yes’ 로 변경
PermitRootLogin yes
# 저장후, ssh server restart
$ sudo service ssh restart
```

<br/>

### 6.4. root 권한 생성.

```sh
$ sudo passwd root
```

<br/>

### 6.5. 리눅스 스왑 제거.

※ 스왑이 설정되어 있으면 Kubernetes 설치가 안되고 오류가 납니다

```sh
# root 권한으로 실행
$ swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab
```

<br/>

### 6.6. NTP 설치.

master 와 node 서버가 시간이 맞지 않으면 kubernetes 가 비정상 동작을 합니다 .

이를 막기 위해서 아래와 같이 ntp 를 설치 해 줍니다

```sh
$ sudo apt install -y ntp
# 또는 root 계정으로
$ apt install -y ntp
```

### 6.7. 고정 IP Setting

※ 우분투 리눅스를 설치하면 기본으로 VirtualBox DHCP 에서 IP 를 받기 때문에 고정 IP 을 할당 해 줍니다

```sh
# 1. 현재 gateway 확인
$ route
or
$ netstat r

# 2. 현재 네트웍 설정 확인
$ vi /etc/netplan/50-cloud-init.yaml

# 3. 네트웍 설정 디렉토리 이동
$ cd /etc/netplan

# 4. 현재 설정 파일 복사
$ cp -arp 50-cloud-init.yaml 01-netcfg.yaml

# 5. 복사한 설정 파일 편집
$ vi /etc/netplan/01-netcfg.yaml

# [예시]
#
# 고정 IP 설정시 'dhcp4: no' 를 꼭 추가
#
# 참고 : 01-netcfg.yaml 에 설정하지 않고 50-cloud-init.yaml 직접 설정하는 이유는
# 이렇게 할 경우가 네트웍 오류 발생 확률이 적은 것 같음
#
# 본 예제는 01-netcfg.yaml 에서 셋팅.
#
# 복사한 01-netcfg.yaml 파일 셋팅 후 사용시 네트워크 에러가 발생하는 경우는
# 50-cloud-init.yaml 파일에 직접 셋팅.
network:
  ethernets:
    enp0s3:
      dhcp4: no
      addresses: [10.0.2.200/24]
      gateway4: 10.0.2.0
    enp0s8:
      dhcp4: no
      addresses: [192.168.56.200/24]
      gateway4: 192.168.56.0
      nameservers:
        addresses: [8.8.8.8,8.8.4.4]
  version: 2

# 5. 고정 IP 설정시 dhcp4: no 를 꼭 추가

# 6. 저장하고 빠져 나옴
# vi 에디터에서 root 권한 저장
$ :w !sudo tee %

# 7. 네트웍 변경 사항 반영
$ netplan apply

# 8. 설정된 변경사항 확인
$ ip addr
$ ip route

# 9. HOST Name 이 kubemaster 가 아닌 다른 이름으로 되어 있다면 변경해 줍니다
#    아래 파일이 있는지 확인 합니다 .
$ vi /etc/cloud/cloud.cfg
# Preserve_hostname: false 로 되어있으면 preserve_hostname : true 로 변경
$ hostnamectl set-hostname kubemaster
$ hostnamectl
# "Static hostname: kubemaster" 로 나오면 성공

# 10. 사전준비에서 다운로드 받아 설치한 MobaXterm 으로 접속 확인.

# 11. 구글 DNS 가 정상적으로 나오는지 확인
$ nslookup google.com
```

<br/>

## 7. Docker 설치

<br/>

```sh
# 1. HTTPS 를 통해 repositor 를 사용할 수 있도록 패키지 설치 (root 권한으로 실행)
#$ apt-get update
#$ apt-get install apt-transport-https ca-certificates curl software-properties-common
$ apt update
$ apt install apt-transport-https ca-certificates curl software-properties-common

# 2. Docker의 공식 GPG 키 추가 (root 권한으로 실행)
$ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -

# 3. Docker apt 리포지터리 추가 (root 권한으로 실행)
$ add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

# 4. Docker CE설치 (root 권한으로 실행)
#$ apt-get install docker-ce=18.06.3~ce~3-0~Ubuntu
$ apt install docker-ce=18.06.3~ce~3-0~Ubuntu

# 5. Docker 가 업데이트 안되도록 설정 (root 권한으로 실행)
$ apt-mark hold docker-ce

# 6. 데몬 설정 (root 권한으로 실행)
$ mkdir /etc/docker

$ cat > /etc/docker/daemon.json <<EOF
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m"
  },
  "storage-driver": "overlay2"
}
EOF

$ mkdir -p /etc/systemd/system/docker.service.d

# 7. Docker 재시작
$ systemctl daemon-reload
$ systemctl restart docker

# 8. Dorker 버전 확인 아래와 같이 나오면 정상
$ docker -v
Docker version 18.06.3-ce, build d7080c1
```

<br/>

## 8. 가상 머신 복제

<br/>

위 과정에서 만들어진 `'kubemaster'` 가상 머신을 각각 `'kubenode01'`, `'kubenode02'`, `'kubenode03'`, `'vmdemo'` 4개 더 복제 한다.

각 가상 머신의 역할은 다음과 같다.

- kubemaster : Kubernetes Master Node
- kubenode01 : Kubernetes Worker Node1
- kubenode02 : Kubernetes Worker Node2
- kubenode03 : Kubernetes Worker Node3
- vmdemo : Docker

각의 역할을 하는 가상 머신을 복제한 후 `'6.7. 고정 IP Setting'` 과정을 반복 한다.

이때 각의 머신에 맞게 머신별 `'ip'`, `'hostname'` 을 변경해야 한다.

각 머신의 ip, hostname 은 아래와 같다.

- kubemaster <br/>
  hostname : kubemaster <br/>
  enp0s3 ip : 10.0.2.200 <br/>
  enp0s8 ip : 192.168.56.200

- kubenode01 <br/>
  hostname : kubenode01 <br/>
  enp0s3 ip : 10.0.2.201 <br/>
  enp0s8 ip : 192.168.56.201

- kubenode02 <br/>
  hostname : kubenode02 <br/>
  enp0s3 ip : 10.0.2.202 <br/>
  enp0s8 ip : 192.168.56.202

- kubenode03 <br/>
  hostname : kubenode03 <br/>
  enp0s3 ip : 10.0.2.203 <br/>
  enp0s8 ip : 192.168.56.203

- vmdemo <br/>
  hostname : vmdemo <br/>
  enp0s3 ip : 10.0.2.220 <br/>
  enp0s8 ip : 192.168.56.220

각 머신의 복제 및 ip 셋팅이 끝나면, `'MobaXterm'` 으로 각 머신에 ssh 접속 테스트를 한다.

<br/>

## 9. Master Node 에 Kubernetes 설치.

<br/>

```sh
# 1. HTTPS 를 통해 repository 를 사용할 수 있도록 패키지 설치
$ apt update
$ apt install -y apt-transport-https

# 2. 구글의 신뢰할 수 있는 키를 추가 및 Kubernetes 설치 리스트 가져오기
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ cat <<EOF >/etc/apt/sources.list.d/kubernetes.list
deb http://apt.kubernetes.io/ kubernetes-xenial main
EOF

# 3. 최신 버전 패키지가 있는지를 확인하고 Kubernetes 패키지 설치
$ apt update
$ apt install -y kubelet kubeadm kubectl

# 4. kubernetes 도 마찬가지로 업데이트되지 않도록 hold 걸어 놓습니다 .
$ apt-mark hold kubelet kubeadm kubectl

# 5. 시스템 데몬 재시작
$ systemctl daemon reload
$ systemctl restart kubelet

# 5 번 항목 시작시 별다른 에러가 나지 않는 다면 6 번 항목은 Pass 한다

# 6. kubelet 에서 사용하는 cgroup 드라이버 구성 (옵션 사항)
# 도커 cgroup drive 확인
$ docker info | grep -I cgroup
# /etc/systemd/system/kubelet.service.d/10-kubeadm.conf 라인추가
# Environment="KUBELET_CGROUP_ARGS=–cgroup-driver=cgroupfs"
# or
# Environment="KUBELET_CGROUP_ARGS=–cgroup-driver=systemd"
# cgroup driver 변경
$ sed -i "s/cgroup-driver=systemd/cgroup-driver=cgroupfs/g" /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

# 컨테이너는 Linux Namespaces, cgroups 두 가지 매커니즘을 통하여 프로세스를 격리, 동작하게 합니다.
# 기본적으로 kubeadm이 자동으로 cgroup driver를 찾아주지만, 다른 CRI를 사용한다면 수동으로 이를
# /etc/default/kubelet에 등록해주어야 합니다.

# ※ Kubernetes 시작시 별다른 오류가 없다면 이 부분은 skip 해도 됩니다

# 도커와 Kubernetes 의 cgroup 의 드라이버 구성이 맞지 않으면 아래와 같은 에러 발생
# failed to run Kubelet: failed to create kubelet: misconfiguration: kubelet cgroup driver:
# "cgroupfs" is different from docker cgroup driver: "systemd"

# 7. kubernetes Master 구성
#   - advertise-address : Master Node의 Ip를 입력한다
#   - pod-network : pod들이 사용할 내부 네트워크 대역을 입력한다. (무조건 10.244.0.0/16 이어야 함!)
# kubeadm init 명령 실행하기 전 스왑 정지 및 영구 삭제 명령 실행
$ swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab

$ kubeadm init --apiserver-advertise-address=192.168.56.200 --pod-network-cidr=10.244.0.0/16

# 위 명령을 실행후 아래와 같은 화면이 나오면 정상
#
# Your Kubernetes control-plane has initialized successfully!
#
# To start using your cluster, you need to run the following as a regular user:
#
#   mkdir -p $HOME/.kube
#   sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
#   sudo chown $(id -u):$(id -g) $HOME/.kube/config
#
# You should now deploy a pod network to the cluster.
# Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
#   https://kubernetes.io/docs/concepts/cluster-administration/addons/
#
# Then you can join any number of worker nodes by running the following on each as root:
#
# ... 아래 명령은 worker node 가 join 시 사용 할 명령임으로 반드시 다른데 복사해 놓아야 합니다 ...
# ... init 명령 실행시 token 값과 sha256 값은 항상 변합니다 ...
#
# kubeadm join 10.0.2.200:6443 --token c0ljgo.cmy2v846a698e1b2 \
#     --discovery-token-ca-cert-hash sha256:4009869662678f63b8e2d8de9b9b7841c9532ebac1c0ff36bc0de4e7f3cf4cbe
#
# 위와 같은 화면이 나오면 Master node 설정은 완료 된 것입니다
# token 값과 sha256 해쉬값 설치시 마다 다른 값이 나옵니다

# 8. Master node Kubernetes 명령어 사용 환경 구성
# kubectl 명령어를 사용하기 위한 인증 파일 복사도 한다.(일반 유저로)
# 일반 User 로 다시 빠져 나옵니다
$ mkdir -p $HOME/.kube
$ sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
$ sudo chown $(id -u):$(id -g) $HOME/.kube/config

# 9. calico 설치
# calico 3.8 설치 
# calico 3.8 network 설치 전 아래 폴더가 있을 경우 Ready 상태가 안된다.
$ rm -rf /var/lib/cni/
$ kubectl apply -f https://docs.projectcalico.org/v3.8/manifests/calico.yaml
# calico 3.5 설치 
# Installing with the Kubernetes API datastore—50 nodes or less
$ kubectl apply -f https://docs.projectcalico.org/v3.5/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/1.7/calico.yaml
or
# Installing with the Kubernetes API datastore—more than 50 nodes
$ kubectl apply -f https://docs.projectcalico.org/v3.5/getting-started/kubernetes/installation/hosted/kubernetes-datastore/calico-networking/typha/calico.yaml

# 10. Master node 동작 확인
# 설치후 아래와 같이 msater node가 Ready 상태로 나와야 한다.
$ kubectl get nodes
NAME        STATUS   ROLES    AGE     VERSION
kubemater   Ready    master   5m43s   v1.13.2
```

<br/>

## 10. Worker Node 에 Kubernetes 설치.

<br/>

Worker Node Kubernetes 설치는 위에서 작업한 `'Master Node 에 Kubernetes 설치'` 에서 작업한 내용과 대부분 동일하다.

`'Master Node 에 Kubernetes 설치'` 에서 1~6번 항목은 동일한 작업을 수행 후 아래 작업을 추가로 한다.

```sh
# kubeadm join 명령 실행하기 전 스왑 정지 및 영구 삭제 명령 실행
$ swapoff -a && sed -i '/swap/s/^/#/' /etc/fstab

# kubernetes Worker 구성
# master node init 명령에서 복사해 놓은 명령어를 입력해야 됩니다.
$ kubeadm join 10.0.2.200:6443 --token c0ljgo.cmy2v846a698e1b2 \
     --discovery-token-ca-cert-hash sha256:4009869662678f63b8e2d8de9b9b7841c9532ebac1c0ff36bc0de4e7f3cf4cbe
```

<br/>

## 11. 참고 사항

<br/>

```sh
# 1. Worker node join 시 오류 발생 할 경우
#    Work node 에서 'kubeadm reset' 명령으로 'join' 을 초기화 할 수 있습니다
$ kubeadm reset
#    그리고 나서 복사해둔 아래 명령을 Worker node 에서 다시 실행 시켜 줍니다
$ kubeadm join 10.0.2.200:6443 --token c0ljgo.cmy2v846a698e1b2 \
     --discovery-token-ca-cert-hash sha256:4009869662678f63b8e2d8de9b9b7841c9532ebac1c0ff36bc0de4e7f3cf4cbe

# 2. Kubeadm join 명령 실행시 token 값과 sha256 값이 안 먹거나 잃어 버린 경우
#    Master node 로 이동하여 발행된 token 을 확인 합니다
$ kubeadm token list
#    token을 가져오거나 or 생성, generate 명령은 token 을 가지고 오지만 새로 생성하지는 않습니다
$ kubeadm token generate (or create) token 유효 기간의 지났을 경우 token 을 새로 생성 해야 합니다
#    sha256 Hash 값을 확인
$ openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex |
sed 's/^.*//'
```
