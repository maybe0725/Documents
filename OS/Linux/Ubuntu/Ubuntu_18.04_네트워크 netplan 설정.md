# Ubuntu Bionic Netplan

<br/>

> 출처 - https://judo0179.tistory.com/43?category=294005

<br/>

## 설치 타입에 따른 설정파일 위치

| Install Type | File Location                            |
| ------------ | ---------------------------------------- |
| Server ISO   | /etc/netplan/01-netcfg.yaml              |
| Cloud Image  | /etc/netplan/50-cloud-init.yaml          |
| Desktop ISO  | /etc/netplan/01-network-manager-all.yaml |

설치 타입에 따라 파일명은 달라질 수 있으나 기본적인 디렉토리 위치는 같다.

또한 특이점으로는 YAML 표기법으로 변경되었다.
