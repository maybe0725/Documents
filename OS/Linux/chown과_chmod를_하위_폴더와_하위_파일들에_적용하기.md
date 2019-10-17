# chown과 chmod를 하위 폴더와 하위 파일들에 적용하기

<br/>

둘다 공통적으로 `-R` 옵션을 적용해주면 된다.

<br/>

## chmod

<br/>

```sh
$ chmod -R [8bit permission] [file name or folder name]
```

예시

```sh
### example의 하위 폴더와 파일들에 권한을 666(-rw-rw-rw-)로 변경합니다.
$ chmod -R 666 example
```

<br/>

## chown

<br/>

```sh
$ chown -R [owner name]:[group name] [filename or directory]
```

예시

```sh
### example의 하위 폴더와 파일들에 소유자를 sam으로 그룹을 abbey로 설정합니다.
$ chown -R sam:abbey example
```
