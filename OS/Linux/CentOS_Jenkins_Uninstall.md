# CentOS-8 Jenkins Uninstall

<br/>

## 1. 젠킨스 데몬 죽이기

```sh
[root@maybe-test ~]# service jenkins stop
Stopping jenkins (via systemctl):                          [  OK  ]
[root@maybe-test ~]# /etc/init.d/jenkins stop
Stopping jenkins (via systemctl):                          [  OK  ]
```

<br/>

## 2. jenkins Uninstall

```sh
[root@maybe-test ~]# yum remove jenkins
종속성이 해결되었습니다.
=================================================================================================================================
 꾸러미                       아키텍처                    버전                               리포지토리                     크기
=================================================================================================================================
삭제 중:
 jenkins                      noarch                      2.190.1-1.1                        @jenkins                       75 M

거래 요약
=================================================================================================================================
삭제  1 꾸러미

자유 공간 : 75 M
이게 괜찮습니까 [y / N] : y
트랜잭션 점검 실행 중
트랜잭션 검사가 성공했습니다.
트랜잭션 테스트 실행 중
트랜잭션 테스트가 완료되었습니다.
거래 실행 중
  준비 중입니다  :                                                                                                           1/1
  스크립틀릿 실행: jenkins-2.190.1-1.1.noarch                                                                                1/1
  삭제 중        : jenkins-2.190.1-1.1.noarch                                                                                1/1
경고: /etc/sysconfig/jenkins(이)가 /etc/sysconfig/jenkins.rpmsave(으)로 저장되었습니다

  스크립틀릿 실행: jenkins-2.190.1-1.1.noarch                                                                                1/1
  확인 중        : jenkins-2.190.1-1.1.noarch                                                                                1/1

제거됨:
  jenkins-2.190.1-1.1.noarch

완료되었습니다!
```

<br/>

## 3. jenkins 제거

```sh
[root@maybe-test ~]# rm /etc/init.d/jenkins
rm: cannot remove '/etc/init.d/jenkins': 그런 파일이나 디렉터리가 없습니다
[root@maybe-test ~]# rm -rf /var/lib/jenkins
[root@maybe-test ~]# rm /etc/yum.repos.d/jenkins.repo
rm: remove 일반 파일 '/etc/yum.repos.d/jenkins.repo'? y
```
