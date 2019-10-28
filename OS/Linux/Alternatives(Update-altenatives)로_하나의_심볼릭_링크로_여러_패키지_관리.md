# Alternatives(Update-altenatives)로 하나의 심볼릭 링크로 여러 패키지 관리

<br/>

[Alternatives(Update-altenatives)로 하나의 심볼릭 링크로 여러 패키지 관리](http://no1linux.org/hottips/28242)

<br/>

버전이 각기 다른 패키지에 대해서 동일한 심볼릭 링크를 사용해야 할 경우에 어떻게 해야 할까?

원하는 버전으로 실행하고자 한다면 매번 해당 버전으로 심볼릭 링크를 걸어주면 될 것입니다.

사용자가 아닌 여러 프로그램에서 각기 다른 버전을 사용한다면 사용자가 심볼릭 링크를 변경해 주지 않고서는 프로그램이 자동으로 적당한 버전으로 심볼릭 링크를 해 주기는 어렵습니다.

예를 들어 python을 생각해 보죠. 파이썬은 현재 pythion2버전과 python3 버전이 시스템에 동시에 깔려 있습니다.

파이썬이 현재 2버전으로 심볼릭링크되어 있는데, 다른 파이썬 프로그램이 3버전을 요구할 경우 python 명령이 실행되어질 때 2버전으로 동작하기 때문에 3버전으로 심볼릭 링크를 갱신해 주지 않는 한 오류가 발생될 수 있습니다.

여기서 딜레마가 발생할 수 있습니다.

스크립트로 3버전을 요구하는 프로그램에 대해서 3버전으로 심볼릭 링크 처리해 주면 동작하겠지만, 2버전을 요구하는 다른 프로그램에선 또 오류가 발생될 수 있습니다.

이러한 문제를 해결하기 위해서 고안된 것이 alternatives 도구로 데비안 리눅스에서 개발되었고, 넘버원 리눅스와 레드햇 리눅스 계열에서는 update-alternatives 명령을 사용하며 alternatives로 심볼릭링크되어 있으므로, 두 명령어중 한 명령으로 사용이 가능합니다.

update-alternatives를 이용하면 여러 개의 패키지에 대해서 한 개의 심볼릭 링크를 통하여 관리할 수 있는 이점이 있는데, 오늘의 강좌에서는 이 명령어의 사용법에 대해서 알아보는 시간을 갖도록 하겠습니다.

<br/>

## Update-alternatives 사용법

<br/>

```sh
root@ubuntu-deploy:~# update-alternatives --help
Usage: update-alternatives [<option> ...] <command>

Commands:
  --install <link> <name> <path> <priority>
    [--slave <link> <name> <path>] ...
                           add a group of alternatives to the system.
  --remove <name> <path>   remove <path> from the <name> group alternative.
  --remove-all <name>      remove <name> group from the alternatives system.
  --auto <name>            switch the master link <name> to automatic mode.
  --display <name>         display information about the <name> group.
  --query <name>           machine parseable version of --display <name>.
  --list <name>            display all targets of the <name> group.
  --get-selections         list master alternative names and their status.
  --set-selections         read alternative status from standard input.
  --config <name>          show alternatives for the <name> group and ask the
                           user to select which one to use.
  --set <name> <path>      set <path> as alternative for <name>.
  --all                    call --config on all alternatives.

<link> is the symlink pointing to /etc/alternatives/<name>.
  (e.g. /usr/bin/pager)
<name> is the master name for this link group.
  (e.g. pager)
<path> is the location of one of the alternative target files.
  (e.g. /usr/bin/less)
<priority> is an integer; options with higher numbers have higher priority in
  automatic mode.

Options:
  --altdir <directory>     change the alternatives directory.
  --admindir <directory>   change the administrative directory.
  --log <file>             change the log file.
  --force                  allow replacing files with alternative links.
  --skip-auto              skip prompt for alternatives correctly configured
                           in automatic mode (relevant for --config only)
  --verbose                verbose operation, more output.
  --quiet                  quiet operation, minimal output.
  --help                   show this help message.
  --version                show the version.
root@ubuntu-deploy:~#
```

<br/>

### 1. 심볼릭링크 생성(--install)

```
update-alternatives --install <link> <name> <path> <priority>  [--slave <link> <name> <path>] ...
```

```
<link>     : 심볼릭링크명이 위치할 경로명
<name>     : /etc/alternatives 경로에 위치할 심볼릭링크명
<path>     : 패키지가 설치되어 있는 경로
<priority> : 자동모드로 동작할 때 동작할 우선 순위
```

예제로 NVidia 그래픽 카드 드라이버의 ld.so.conf 에 대해서 gl_conf로 심볼릭 링크를 생성해 봅니다.

먼저 기존 심볼릭 링크를 제거하고 새로 생성해 봅니다.

```sh
### 링크제거
update-alternatives --remove gl_conf /etc/nvidia-current/ld.so.conf
update-alternatives --remove gl_conf /etc/nvidia340/ld.so.conf
update-alternatives --remove gl_conf /etc/nvidia304/ld.so.conf
```

자, 그러면 다음과 같이 nvidia-current(nvidia 390 드라이버)의 ld.so.conf에 대해서 gl_conf로 심볼릭 링크를 추가합니다.

```sh
### 링크생성
update-alternatives --install /etc/alternatives/ld.so.conf gl_conf /etc/nvidia-current/ld.so.conf 500
```

그 다음에도 nvidia340, nvidia304에 대해서 다음과 같이 심볼릭 링크를 합니다.

```sh
### 링크생성
update-alternatives --install /etc/alternatives/ld.so.conf gl_conf /etc/nvidia340/ld.so.conf 400
update-alternatives --install /etc/alternatives/ld.so.conf gl_conf /etc/nvidia304/ld.so.conf 300
```

<br/>

### 2. 심볼릭링크 수동모드및 확인(--config)

```
update-alternatives --config <name>
```

여러 개의 패키지에 대해서 alternatives로 심볼릭링크를 걸어 놓았을 때 `심볼릭링크 상태를 확인하거나` 심볼릭링크 `우선순위를 변경하고자 할 때`는 `--config` 옵션을 사용합니다.

그러면 이 옵션을 사용하여 엔비디아 그래픽 카드의 ld.so.conf 파일에 대한 심볼릭 링크 목록을 확인해 봅니다.

```sh

update-alternatives --config gl_conf

 There are 4 programs which provide `gl_conf'.

  Selection    Command
-----------------------------------------------
*+    1        /etc/ld.so.conf.d/GL/standard.conf
      2        /etc/nvidia-current/ld.so.conf
      3        /etc/nvidia340/ld.so.conf
      4        /etc/nvidia304/ld.so.conf
Enter to keep the default[*], or type selection number:
```

네 개의 목록이 있습니다.

앞서 심볼릭링크를 추가할 때 nvidia-current를 500, nvidia340은 400, nvidia304은 300으로 우선순위도를 지정해 주었는데, 숫자가 높을수록 우선순위도가 높습니다.

현재 시스템에서 기본적으로 설정되어 있는 1번이 가장 높은 우선순위도로 되어 있습니다.

그러면 /etc/alternatives/gl_conf 심볼릭 링크의 상태를 확인해 봅니다.

```sh
ll /etc/alternatives/gl_conf
lrwxrwxrwx 1 root root 34  4월 16 17:55 /etc/alternatives/gl_conf -> /etc/ld.so.conf.d/GL/standard.conf
```

현재 /etc/ld.so.conf.d/GL/standard.conf 파일이 gl_conf로 심볼릭링크되어 있습니다.

그러면 `--config` 옵션을 넣어 실행하여 기본값으로 3를 입력해 봅니다.

```sh
update-alternatives --config gl_conf

There are 4 programs which provide `gl_conf'.

  Selection    Command
-----------------------------------------------
*+    1        /etc/ld.so.conf.d/GL/standard.conf
      2        /etc/nvidia-current/ld.so.conf
      3        /etc/nvidia340/ld.so.conf
      4        /etc/nvidia304/ld.so.conf

Enter to keep the default[*], or type selection number:3

Using `/etc/nvidia340/ld.so.conf' to provide `gl_conf'.

ll /etc/alternatives/gl_conf
lrwxrwxrwx 1 root root 25  4월 16 18:06 /etc/alternatives/gl_conf -> /etc/nvidia340/ld.so.conf
```

gl_conf 심볼릭링크가 /etc/nvidia340/ld.so.conf로 변경되었음을 확인할 수 있습니다.

이렇게 --config 옵션을 사용하여 수동으로 원하는 파일로 심볼릭링크를 갱신할 수 있습니다.

<br/>

### 3. 자동 모드 (--auto)

```
update-alternatives --auto <name>
```

자동모드는 우선순위도 값에 따라서 심볼릭링크가 결정되는데, 제일 높은 값이 먼저 선택되며, 자동으로 선택된 링크가 제거되면 다음 낮은 순위로 심볼릭링크가 갱신됩니다.

즉 세개의 엔비디아 그래픽 카드 드라이버만 있을 경우에 제일 순위도가 높은 dkms-nvidia-current 패키지가 제거되면 자동으로 dkms-nvidia-340 패키지가 gl_conf로 심볼릭링크 되는 것입니다.

<br/>

### 4. 심볼릭링크 제거(--remove)

```
update-alternatives --remove <name> 경로명/파일명
```

이 옵션에 대해서는 --install 옵션에 살펴보았듯이, 삭제하고자 하는 심볼릭링크 대상파일명을 지정해서 제거하면 됩니다.

<br/>

### 5. 심볼릭링크 목록 확인( --display)

```
update-alternatives --display <name>
```

alternatives로 설정된 심볼릭링크 목록을 확인하고자 할 때는 `--display` 옵션을 사용합니다.

```sh
update-alternatives --display gl_conf
gl_conf - status is manual.
  link currently points to /etc/nvidia340/ld.so.conf
/etc/ld.so.conf.d/GL/standard.conf - priority 500
  slave xorg_extra_modules: /usr/lib64/xorg/xorg-1.6-extra-modules
/etc/nvidia-current/ld.so.conf - priority 500
/etc/nvidia340/ld.so.conf - priority 400
/etc/nvidia304/ld.so.conf - priority 300
Current `best' version is /etc/ld.so.conf.d/GL/standard.conf.
```

`--list` 옵션을 사용하여 간단하게 심볼릭링크 목록을 확인할 수도 있습니다.

```sh
# update-alternatives --list gl_conf
/etc/ld.so.conf.d/GL/standard.conf
/etc/nvidia-current/ld.so.conf
/etc/nvidia340/ld.so.conf
/etc/nvidia304/ld.so.conf
```

<br/>

### 6. 심볼릭링크 변경하기(--set)

```
update-alternatives --set <name> <path>
```

`--config` 옵션으로 심볼릭링크를 변경하는 방법에 대해서 알아 보았는데, `--config` 옵션은 목록을 확인하여 원하는 대상으로 심볼릭링크를 거는 것에 비해 `--set` 옵션은 심볼릭링크 목록을 알고 있는데 상태에서 바로 원하는 대상으로 심볼릭링크를 갱신하고자 할 때 사용됩니다.

`--config` 옵션은 스크립트에 적용하기 어려우나, 이 옵션은 스크립트내에선 유용합니다.

```sh
update-alternatives --set gl_conf /etc/nvidia-current/ld.so.conf
Using `/etc/nvidia-current/ld.so.conf' to provide `gl_conf'.
```
