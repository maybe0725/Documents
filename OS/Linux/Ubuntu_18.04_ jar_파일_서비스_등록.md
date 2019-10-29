# Ubuntu 18.04 jar 파일 서비스 등록(Ubuntu service jar file)

<br/>

### 1. 서비스 파일을 작성한다. (root로 하거나 sudo 명령어를 입력하여 작성하도록 한다)

- sudo vi /etc/systemd/system/myapp.service 또는 sudo nano /etc/systemd/system/myapp.service
- 파일이름 myapp.service는 임으로 정한 이름이다.
- 파일 전송기를 통해서 전송 하여도 된다.

<br/>

### 2. 아래 내용을 입력한다.

```sh
#myapp.service
[Unit]
Description=Kisa scheduler Service
After=network.target

[Service]
ExecStart=/bin/bash -c "exec java -jar /동작시킬 jar파일 위치/파일명.jar"

User=root
Group=root

[Install]
WantedBy=multi-user.target
```

<br/>

### 3. 서비스에 등록한다.

```
sudo systemctl enable myapp.service
```

<br/>

### 4. 실제로 동작하는지 명령어를 통해 확인한다.

```
sudo service myapp start
```

<br/>

### 5. 재부팅해서도 잘 동작 하는지 확인하여 본다.

```
reboot
```
