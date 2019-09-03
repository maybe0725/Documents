# Vue의 Nuxt로 개발할 떄 자동으로 브라우저 띄우는 방법

<br/>

원본글 출처

- [Vue의 Nuxt로 개발할 떄 자동으로 브라우저 띄우는 방법](https://ux.stories.pe.kr/148)

<br/>

Nuxt로 Vue를 개발하다 보면 항상 사용하는 명령어가 npm run dev 또는 npm dev 또는 yarn run dev 또는 yarn dev 일 것입니다.
모두 동일하게 `Nuxt를 개발 모드로 띄워라` 인데.. 몇가지 명령어를 덫붙히면 dev모드 실행과 동시에 브라우저를 자동으로 띄 울 수 있는 몇가지 방법이 있습니다.

<br/>

## 첫번째 방법

<br/>

아주 간단한 첫번째 방법은 `package.json`을 사용하는 방법입니다.

아래와 같이 package.json의 scripts명령어 중에 원하는 실행 명령 뒤에 "dev": "nuxt --open"와 같이 --open을 붙히는 것입니다.

```json
{
  //...
  "scripts": {
    "dev": "nuxt --open",
    "build": "nuxt build",
    "start": "nuxt start",
    "generate": "nuxt generate",
    "lint": "eslint --ext .js,.vue --ignore-path .gitignore ."
  }
  //...
}
```

아래와 같이 실행명령을 치면 로딩 후 자동으로 기본 브라우저가 오픈이 됩니다.

```sh
$ yarn dev
```

<br/>

## 두번째 방법

<br/>

첫번째 방법의 아쉬운 점은 내가 원하는 브라우저를 띄울 수 없다는 것입니다. 시스템에 기본으로 설정이 되어 있는 브라우저를 띄워주게 됩니다. 예를 들어 Mac를 사용할 경우 크롬브라우저를 띄워주는 것이 아니라 기본 브라우저인 사파리를 띄워 줍니다.

그래서 두번째 방법은 내가 원하는 브라우저를 띄우는 방법을 설명드리겠습니다.
이것은 opn이라는 모듈을 사용해야 합니다.

<br/>

### 1. opn 모듈 설치

현재 Nuxt프로젝트가 있다는 가정하에 해당 프로젝트에 opn을 devDependencies로 설치해 줍니다.

opn은 말 그대로 open의 역활을 해 줍니다.

```sh
$ yarn add -D opn
```

opn모듈에는 원하는 종류의 브라우저를 옵션을 포함해서 띄워 주는 기능이 있습니다.

간단하게 명령을 설명하면 아래와 같습니다.

```js
// 파이어폭스 브라우저로 네이버를 띄워줌
opn("http://naver.com", { app: "firefox" });
```

좀더 자세한 정보는 [opn모듈 github](https://github.com/sindresorhus/open)에서 보시면 됩니다.

<br/>

### 2. nuxt.config.js 에 사용 설정하기

이제 설치된 opn모듈을 설정합니다. opn설정은 package.json이 아니라 `nuxt.config.js`에서 해 줘야 합니다.

const opn = require('opn')으로 opn모듈을 opn변수에 담습니다.

그리고 nuxt의 hooks를 이용하여 호스트의 변화를 보고(listen)있다가 변경이 있다면 opn을 실행할 수 있게 해줍니다.

```js
const opn = require("opn");

export default {
  hooks: {
    listen(server, { host, port }) {
      // yarn dev 실행 시 자동으로 크롬 브라우져를 띄워줌 (incognito는 시크릿모드로 실행)
      opn(`http://${host}:${port}`, { app: ["google chrome", "--incognito"] });
    }
  }
};
```

Nuxt를 개발할 때 필수 기능은 아니지만 편리함을 더해 주는 브라우저 자동으로 띄우기 였습니다.
