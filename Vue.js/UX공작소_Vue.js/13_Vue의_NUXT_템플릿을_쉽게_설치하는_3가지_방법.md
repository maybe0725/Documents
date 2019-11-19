# Vue의 NUXT 템플릿을 쉽게 설치하는 3가지 방법

<br/>

원본글 출처

- [Vue의 NUXT 템플릿을 쉽게 설치하는 3가지 방법](https://ux.stories.pe.kr/146)

<br/>

Vuejs의 장점은 매우 가볍고 쉽고 빠르게 기존에 있는 웹서비스에 접목해서 사용하는 것입니다.
새로운 웹프로젝트에도 쉽고 빠르게 적용을 할 수 있습니다.
하지만 쉽다고 해도 Vuejs의 최대 성능을 끌어 올려서 처음부터 파일을 만든다는 것은 생각보다 쉽지 않습니다.
더욱이 개발자 마다, 또는 개발할때마다 기본 틀이 달라진다면 혼자 개발할때는 문제가 없지만 통일된 규격을 가지고 있어야 할 대규모 프로젝트를 진행하기에는 어려운 점이 너무나 많습니다.
그래서 React의 NEXT처럼 Vue에서도 NUXT를 제공하고 있습니다.
NUXT는 일반적인 Vue.js 어플리케이션을 정해진 규격에 맞게 만드는 프레임워크라고 할 수 있습니다.

<br/>

## NUXT 설치하기

<br/>

NUXT에 대한 설명은 나중에 포스팅을 하도록 하겠습니다. 여기에서는 간단하게 NUXT로 프로젝트를 생성하는 방법에 대해서 포스팅 하겠습니다.

NUXT를 설치하는 방법은 크게 3가지 방법이 있는데, 수동으로 설치하는 방법, vue-cli로 설치로 설치하는 방법, 그리고 npm(yarn)으로 설치하는 방법입니다.

<br/>

## 1. 수동으로 설치하기

<br/>

수동으로 설치하는 방법은 NUXT의 폴더와 파일의 구성요소를 직접 생성 및 작성을 하는 방법입니다.
여기서는 최소한의 요소를 세팅하는 방법만 설명할 예정입니다.

가장 원시적인 방법이면서 NUXT에 대해 이미 잘 알고 있어야 하는 방법입니다.

<br/>

### 폴더 생성하기

가장 먼저 프로젝트로 사용할 폴더를 하나 만들고 그 폴더 안으로 들어갑니다.

윈도우 탐색기로 폴더를 만들어도 됩니다.
아래의 코드는 CMD창에서 폴더를 만들고 폴더 안으로 들어가는 명령어입니다.

```sh
$ mkdir <project-name>
$ cd <project-name>
```

<br/>

### package.json 파일 생성하기

package.json파일을 하나 만들고 내용을 작성합니다.

아래 내용은 완전 최소한의 내용입니다.

```json
{
  "name": "my-app",
  "scripts": {
    "dev": "nuxt"
  }
}
```

scripts는 나중에 npm의 명령어로 사용할 예정입니다.

<br/>

### 프로젝트에 nuxt 설치하기

이 프로젝트의 핵심인 nuxt를 설치합니다. 설치는 npm 또는 yarn으로 설치를 합니다.
앞에도 언급이 되어 있지만 yarn은 별도로 설치해 줘야 합니다.

```sh
$ npm install --save nuxt

또는

$ yarn add nuxt
```

> npm과 yarn은 패키지를 관리하는 프로그램으로 비슷한 역활을 하는데 근래에는 yarn을 많이 사용하는 추세입니다. <br/>하지만 npm과 달리 yarn은 별도로 설치를 해줘야 합니다.

<br/>

### pages 폴더 생성하기

실제적으로 화면으로 보여질 page가 저장될 폴더입니다.

윈도우 탐색기로 폴더를 만들어도 됩니다.

```sh
$ mkdir pages
```

<br/>

### pages 폴더에 index.vue 파일 생성하기

첫번째 화면으로 사용할 index.vue 파일을 pages폴더에 만들고 아래 내용을 작성합니다.

```html
<template>
  <h1>Hello world!</h1>
</template>
```

<br/>

### 프로젝트 실행하기

다 되었습니다. 아주 간단한 Hello world! 입니다.

```sh
$ npm run dev
```

실행 후 웹브라우저에서 http://localhost:3000 로 접속하면 정상적인 화면을 볼 수 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/99C962385D0A44B402)

<br/>

### 기타

위의 pages폴더 말고도 Assets,Components, Layouts, Middleware, Plugins, Static, Store의 폴더가 있으나 선택사항입니다. 그리고 설정을 도와주는 nuxt.config.js 라는 중요한 파일도 있으나 필수요소는 아닙니다.

위의 폴더와 파일은 나중에 좀더 자세히 포스팅을 하도록 하겠습니다.

<br/>

## 2. Vue-cli로 설치하기

<br/>

두번째 방법은 vue-cli로 Nuxt `starter-template`을 다운받아 설치하는 방법입니다.
`starter-template`은 딱 필요한 구성만 갖추고 있습니다.
먼저 `Vue-cli`가 설치되어 있어야 합니다.
Vue-cli는 `Command Line Interface`의 약자로 NUXT뿐만 아니라 Vue에 대한 전반적인 명령을 CMD창에 키보드로 실행할 수 있게 해주는 기능입니다.

<br/>

### Vue-cli를 전역으로 설치

Vue-cli는 보통 Vue 개발자라면 대부분 전역으로 한번 설치해서 사용합니다.
그래서 이미 설치가 되어 있을 수도 있습니다. 한번 설치한 적이 있다면 건너 띄어도 됩니다.

```sh
$ npm install -g @vue/cli @vue/cli-init
```

Vue-cli를 설치 했다면 이제 Vue-cli 명령어로 NUXT `starter-template`를 다운로드 받아 설치합니다.

```sh
$ vue init nuxt-community/starter-template <project-name>

? Project name vuenuxttest
? Project description Nuxt.js project
? Author juni
```

위와 같이 3개의 질문에 답변을 합니다.

- Project name : 프로젝트의 이름을 작성합니다. 기본으로 폴더의 이름을 제시합니다.
- Project description : 프로젝트의 간략한 설명을 입력합니다.
- Author : 제작작의 아이디나 이름을 입력합니다.

설치가 완료되면 아래와 같은 메시지를 보여줍니다.

```sh
vue-cli · Generated "vuenuxttest".

To get started:

  cd vuenuxttest
  npm install # Or yarn
  npm run dev
```

설치는 금방 끝납니다. 설치가 완료되면 가장 기본적인 실행 방법을 보여 줍니다.

<br/>

### 프로젝트 실행하기

아래 처럼 해당 폴더(vuenuxttest)로 이동합니다.

```sh
$ cd vuenuxttest
```

패키지를 설치합니다.

```sh
$ npm install # Or yarn
```

설치가 완료되면 실행을 합니다.

```sh
$ npm run dev
```

실행 후 웹브라우저에서 http://localhost:3000 로 접속하면 정상적인 화면을 볼 수 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/99AF59385D0A44B41A)

<br/>

## 3. npm 또는 yarn으로 설치하기

<br/>

마지막으로 npm이나 yarn으로 설치하는 방법입니다.

별도로 Vue-cli가 설치되어 있지 않다거나 그냥 npm이나 yarn으로 설치하고자 할 경우 아래의 명령어를 실행하면 됩니다.

특징이라면 Vue-cli로 설치하는 것에 비해서 훨씬 많은 선택지를 줍니다.

아래와 같이 명령어를 입력합니다. yarn으로 설치하는 Code만 작성하도록 하겠습니다.

```sh
$ yarn create nuxt-app nuxt-test
```

위와 같이 명령어를 실행 시키면 아래와 같은 2가지의 질문을 합니다.

```sh
$ yarn create nuxt-app nuxt-test

? Project name nuxt-test
? Project description My fantabulous Nuxt.js project
```

- Project name : 프로젝트 이름 입력
- Project description : 프로젝트 설명 입력

<br/>

### 서버 설정

NUXT는 프론트와 서버를 같이 작업할 수 있습니다.

서버 프레임워크로 어떤 것을 사용할지 선택하는 단계입니다.
보통 설치하지 않거나 express를 많이 사용합니다.

> 화살표키로 이동하고 shift키로 체크하며 Enter키로 선택하여 넘어갑니다.

```sh
? Use a custom server framework (Use arrow keys)

  none
❯ express
  koa
  adonis
  hapi
  feathers
  micro
```

<br/>

### 부가기능 설정

4가지 부가 기능을 설정할 수 있습니다. 여러 개 중복 선택이 가능합니다. 부가 기능은 대부분 Plugin으로 설치가 됩니다.

```sh
? Choose features to install

  ◉ Progressive Web App (PWA) Support
  ◉ Linter / Formatter
  ◉ Prettier
❯ ◉ Axios
```

- Progressive Web App (PWA) Support : 웹과 네이티브의 장점을 가지고 있는 PWA를 지원 할 지 선택
- Linter / Formatter : 코드 검사를 할지 선택
- Prettier : 코딩을 규격에 맞게 할지 선택
- Axios : http를 위한 라이브러리를 설치할지 선택

<br/>

### UI framework 설정

다음으로 UI프레임워크로 무엇을 사용할지 선택 합니다.

프로젝트의 스타일(CSS) 프레임워크를 어느 것으로 활용 할지 선택하면 됩니다.

```sh
? Use a custom UI framework

  none
  bootstrap
❯ vuetify
  bulma
  tailwind
  element-ui
  buefy
```

- none : 아무것도 사용하지 않음
- bootstrap : 알만한 사람은 알고 있는 트위터에서 만든 bootstrap을 사용
- vuetify : 구글이 정의한 Material design을 Vue의 컴포넌트 형태로 제공(Vue프로젝트에서 가장 많이 사용하고 있음)
- bulma : bootstrap과 비슷하나 bootstrap과 달리 javascript를 사용하지 않고 Flexbox를 기반으로 구현한 프레임웍으로 최근 인기가 상승 추세임
- tailwind : 특징을 잘 모르겠음.
- element-ui : 중국에서 개발한 것으로 보이며 깃헙별은 많으나 한국인이 쓰기에는 부족한 점이 많아보임
- buefy : vuetify가 Material design을 Vue의 컴포넌트 형태로 제공하는 것처럼 bulma를 Vue의 컴포넌트 형태로 제공함

Vue프로젝트를 한다면 보통 `vuetify`를 많이 사용하는것 같습니다.

<br/>

### 검사도구 설정

프로젝트 단위 테스트 도구를 선택합니다.
프로젝트를 개발하고 임의의 환경에서 정확한 결과를 도출해 내는지를 확인함으로 오류가 없는지 테스트하는 것이 주요 목적입니다.

```sh
? Use a custom test framework (Use arrow keys)

❯ none
  jest
  ava
```

- none : 아무것도 사용하지 않음
- jest : 페이스북에 만든 테스트 프레임워크
- ava : Mocha에서 AVA로 많이 전향한다고 하는데, 작고 빠르고 간단한 테스트 문법이 장점이라고함.

<br/>

### Single Page 설정

다음은 렌더링을 할 때 일반적인 웹페이지 형태로 할지 아니면 싱글 페이지로 형태로 할지를 선택합니다.

```sh
? Choose rendering mode (Use arrow keys)

❯ Universal
  Single Page App
```

- Universal : 기존의 웹사이트처럼 html 페이지가 여러 개 생성되는 형태로 렌더링 됨
- Single Page App : index 페이지 하나만 생성되고 javascript로 가상의 페이지를 보여주는 형태로 렌더링 됨

<br/>

### 패키지관리자 설정

nodejs로 설치되는 vue의 패키지를 어떤 것으로 관리할 지를 선택합니다.

```sh
  Choose a package manager

  npm
❯ yarn
```

- npm : nodejs를 설치하면 기본적으로 자동 설치되는 npm(node package manager)
- yarn : npm과 동일한 기능을 하나 좀더 성능이 좋다고 함. npm으로 별도로 설치 해야함.

npm이 일반적이나 요즘은 yarn으로 많이 넘어가는 추세임.

이렇게 설정을 하고 Enter를 하면 나머지는 자동으로 설치가 됩니다. 위의 선택에 부합하는 소스를 다운로드받고 npm install을 자동으로 실행해서 관련 패키지들을 설치해 줍니다.

설치가 완료되면 아래와 같은 메시지를 보여줍니다.

```sh
To get started:

  cd nuxt-test
  yarn run dev

To build & start for production:

  cd nuxt-test
  yarn run build
  yarn start

To test:

  cd nuxt-test
  yarn run test

✨ Done in 63.88s.
```

<br/>

### 프로젝트 실행하기

개발버전, Production버전, 테스트버전의 3가지 실행 방법을 보여 줍니다.

공통적으로 일단 아래 처럼 해당 폴더(nuxt-test)로 이동합니다.

```sh
$ cd vuenuxttest
```

<br/>

### 개발 버전 실행 방법

개발 버전은 프로그램을 개발할 때 코드를 수정하면 실시간으로 화면에 반영이 되어 바로바로 확인하며 개발 할 수 있게 해줍니다.

```sh
$ yarn run dev
```

<br/>

### Production 버전 실행 방법

Production버전은 개발이 모두 완료되고 실제로 서비스를 할 때 실행시키는 명령어로 코드를 난독화하고 컴멘트같은 쓸때없는 코드도 제거하는등 최적화된 상태로 만들어서 웹서비스로 띄워 줍니다.

Production버전은 먼저 Build로 파일을 생성한 다음에 start로 서비스를 시작하는 방식으로 진행 합니다.

```sh
$ yarn run build
$ yarn start
```

<br/>

### 테스트 버전을 실행 방법

작성한 코드가 원하는 대로 잘 작동하는지 테스트 해 볼수 있는 기능입니다.

테스트 버전을 실행하면 위에서 선택한 jest 또는 ava 가 실행 됩니다.

```sh
$ yarn run test
```

실행 후 웹브라우저에서 http://localhost:3000 로 접속하면 정상적인 화면을 볼 수 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/99D2404D5D0A44B435)
