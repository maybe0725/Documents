# Github의 취약성 알림 대처 방법 및 Package Library Update

<br/>

참고

- https://blog.sonim1.com/225
- https://blog.outsider.ne.kr/1228
- https://www.npmjs.com/package/npm-check
- 

<br/>
 
## 1. npm update

```sh
$ npm install -g npm
```

<br/>

## 2. npm outdated

<br/>

많은 의존성 모듈에서 새로운 버전이 나오는 모듈을 일일이 확인하기 어려운데
`npm outdated` 명령어를 사용하면 의존성 모듈 중에 `업데이트가 필요한 모듈을 확인할 수 있다.`

- `Current` : 현재 설치된 버전.
- `Wanted` : package.json에 지정한 버전 범위로 설치되는 최대 범위를 의미.<br/>
  npm update를 실행하면 설치되는 버전이다.
- `Latest` : 모듈의 최신 버전이다.

`"Wanted"`와 `"Latest"`가 `같은 모듈`이면 `빨간색`으로 표시되고

`"Latest"`가 `"Wanted"`보다 `높은 모듈`은 `노란색`으로 표시된다.

여기서 확인을 한 뒤에 일일이 원하는 모듈을 업데이트해도 되지만 꽤 귀찮은 일이다.

```sh
# npm use
$ npm outdated

# yarn use
$ yarn outdated

# update
npm update eslint-utils
npm -D install eslint-utils
```

<br/>

## 3. npm-check

<br/>

`npm-check`는 의존성 관리의 불편함을 덜어주는 cli 모듈로
`npm install -g npm-check`를 이용해서 전역으로 설치하면 사용할 수 있다.

npm-check를 실행하면 npm outdated와 비슷한 정보를 볼 수 있지만, 훨씬 풍부한 정보가 나온다.
자세히 보면 앞에서 본 정보와 똑같은데 각 모듈의 상세내용을 볼 수 있는 링크와 업데이트를 위한 npm 명령어로 안내가 나온다.
npm update를 하면 최신 버전이 설치되는 모듈은 `"UPDATE!"`라고 나오고 지정한 버전 범위가 넘는 최신 버전이 있으면 `"NEW VER!"`이라고 표시된다.

기본 설정으로 사용하지 않는 모듈까지 확인해 준다.

사용하지 않는 모듈은 확인하고 싶지 않으면 `--skip-unused` 옵션을 지정하면 된다.

`npm-check`가 편한 점은 인터랙티브 모드를 지원한다는 점이다.
`npm-check -u`를 입력하면 모듈을 선택할 수 있는 화면이 나온다.

잘 만들어진 npm 모듈이라면 `SemVer`를 따르므로 이에 따라 `Patch Update, Minor Update, Non-Semver`를 구분해서 보여준다.

여기서 커서를 이용해서 원하는 모듈을 선택한 뒤 엔터를 누르면 해당 모듈만 업데이트하게 된다.

`npm outdated`로 업데이트할 모듈을 찾아서 업데이트하는 것보다 훨씬 편하게 업데이트할 수 있다.
이렇게 관리를 하면 코드가 깨질 걱정이 없는 패치 버전 업데이트는 자주 업데이트를 할 수 있다.

```sh
# npm-check install
$ npm install -g npm-check

# use
$ npm-check

# 모듈 선택 및 업데이트
$ npm-check -u
```

<br/>
