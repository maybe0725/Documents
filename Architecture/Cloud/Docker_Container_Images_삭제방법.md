# Docker Container Images 삭제방법

## 컨테이너 삭제

### 동작중인 컨테이너 확인

```sh
$ docker ps
```

### 정지된 컨테이너 확인

```sh
$ docker ps -a
```

### 컨테이너 삭제

```sh
$ docker rm [컨테이너id]
```

### 삭제된 것 확인

```sh
$ docker ps -a
```

### 복수개 삭제도 가능

```sh
$ docker rm [컨테이너id], [컨테이너id]
```

### 컨테이너 모두 삭제

```sh
$ docker rm `docker ps -a -q`
```

## 이미지 삭제

### 현재 이미지 확인

```sh
$ docker images
```

### 이미지 삭제

```sh
$ docker rmi [이미지id]
```

### 컨테이너를 삭제하기 전에 이미지를 삭제할 경우

-f 옵션을 붙이면 컨테이너도 강제 삭제

```sh
$ docker rmi -f [이미지id]
```
