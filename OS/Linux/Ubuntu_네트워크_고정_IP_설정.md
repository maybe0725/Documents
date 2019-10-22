# 네트워크 고정 IP 설정

<br/>

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
