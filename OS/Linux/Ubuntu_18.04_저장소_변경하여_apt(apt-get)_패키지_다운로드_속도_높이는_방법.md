# Ubuntu 18.04 저장소 변경하여 apt(apt-get) 패키지 다운로드 속도 높이는 방법

<br/>

[참고] - [Ubuntu 18.04 / 저장소 변경하여 apt-get 패키지 다운로드 속도 높이는 방법](https://www.manualfactory.net/10525)

<br/>

우분투에서 패키지를 업데이트 하거나 설치하면, 미러 서버에서 소프트웨어를 다운로드하여 설치합니다. 

그런데, 그 미러 서버가 멀리 있거나 속도가 느리다면, 패키지를 다운로드하는데 많은 시간이 소요됩니다. 

속도가 너무 느려서 불편하다면 저장소를 지정하여 속도를 빠르게 할 수 있습니다.

저장소 설정은 `/etc/apt/sources.list`에서 합니다. 파일을 텍스트 에디터로 열고

[http://archive.ubuntu.com/ubuntu](http://archive.ubuntu.com/ubuntu)

를 모두 변경합니다. 만약 한국이라면

[http://mirror.kakao.com/ubuntu](http://mirror.kakao.com/ubuntu)

으로 바꿔보세요.

참고로, 지역별 우분투 미러 서버 리스트는 [여기](https://launchpad.net/ubuntu/+cdmirrors)서 확인할 수 있습니다.
