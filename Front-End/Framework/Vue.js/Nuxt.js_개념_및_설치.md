# Nuxt.js 개념 및 설치

<br/>

원본글 출처
- [Nuxt.js 개념부터 설치까지 빠르게 배우기](https://kdydesign.github.io/2019/04/10/nuxtjs-tutorial/)

<br/>

Nuxt.js는 React의 Next.js에서 영감을 받아 진행된 프로젝트로 온전히 겉으로만 본다면 프레임워크이기 때문에 leaning curve가 있을 것 같지만 
사실상 그렇지는 않다. 
Nuxt.js를 설치하고 사용해 본다면 그저 Nuxt.js는 Vue.js를 사용할 때 하나하나 작업해줘야 할 요소들을 생성해주고 처리해주는 것임을 알 수 있다.

<br/>

## Nuxt.js 소개

<br/>

먼저 [Nuxt.js의 공식 사이트](https://nuxtjs.org/guide) 이다. 
Vue.js와 마찬가지로 한국어 문서가 존재하지만 Nuxt.js의 발빠른 업데이트에 한국어 문서가 따라가지 못하는 것 같다. 
Vue.js 역시 동일하지만.. 
그렇다고 번역하시는 분을 욕하는 게 아니라 그만큼 Nuxt.js의 활동이 활발하다는 이야기이다.
아무튼 이러한 이유로 영문본으로 보기를 바란다.

Nuxt.js의 소개를 본다면 이렇게 명시하고 있다.

- 주요 범위는 UI Rendering이며, 클라이언트/서버 배포를 추상화한다.
- Nuxt.js의 목표는 기본 프로젝트 베이스로 사용할 수 있으며 Node.js 기반의 프로젝트를 유연하게 사용할 수 있는 프레임 워크를 만드는 것이다.
- Nuxt.js는 서버 사이드 렌더링 된 Vue.js 응용 프로그램의 개발을 위해 이에 필요한 모든 구성을 사전 설정한다.
- Nuxt.js는 nuxt generate 라는 배포 옵션을 제공한다. nuxt gererate를 통해 vue.js를 정적인 응용 프로그램으로 빌드한다. 우리는 이 옵션이 마이크로 서비스가 있는 웹 응용 프로그램 개발의 다음 단계일 수 있다고 생각한다.
- Nuxt.js는 단일 응용 페이지(SPA)를 신속하게 만들 수 있다.
- Nuxt.js는 비동기 데이터, 미들웨어, 레이아웃 등과 같이 클라이언트 측과 서버 측 사이에서 개발하는 데 도움이 되는 많은 기능을 제공한다.

어려운 말들이 많을 수 있겠지만 아직도 Nuxt.js를 사용을 해야 할지 말아야 할지 고민하는 당신을 위해 쉽게 설명을 해본다면 이렇다.

- Nuxt.js 설치만으로 이미 scaffolding(프로젝트 구조화)을 해주므로 딱히 프로젝트 구조에 대해서 고민할 필요가 없다.
- Vue.js 하나하나 잡아줘야 할 라우팅을 Nuxt.js에서 파일을 생성하는 것만으로 라우팅을 자동으로 생성해 준다.
- layout, store, middleware와 같은 요소들을 이미 구분을 지어주고 필요한 항목들을 처리해주기 때문에 순전히 개발에만 집중하면 된다.
- Server-Side-Rendering에 필요한 요소가 이미 준비가 되어있다.
- webpack을 통한 빌드 시스템이 이미 구현되어 있다. 그저 npm run만 해주면 된다.

<br/>

## Nuxt.js 설치

<br/>

Nuxt.js 팀이 스캐폴딩 해둔 템플릿을 사용하려면 create-nuxt-app을 npx를 통해서 설치하면된다. 
`npx`는 NPM 5.2.0 버전 이후 기본적으로 제공되기 때문에 별도로 설치할 필요는 없다.

<br/>

## npx를 통한 Nuxt.js 설치

<br/>

```sh
$ npx create-nuxt-app <project-name>
```

위와 같이 실행을 하게 되면 몇 가지 질문들이 나오는데 진행하는 프로젝트에 맞게 선택하여 설치하면 된다.

<br/>

## vue-cli를 통한 Nuxt.js 설치

Nuxt.js 버전이 올라가기 전에는 nuxt starter-template을 vue-cli를 통해서 설치하였다. CLI가 아닌 ZIP 파일로 받아 압축을 풀어도 된다.

먼저 vue-cli를 설치하자. vue-cli에 대해서는 [Vue CLI 공식 사이트](https://cli.vuejs.org/guide/)를 참고하자.

```sh
$ npm i -g @vue/cli
$ npm i -g @vue/cli-init
```

그다음으로 template를 설치하고 npm module을 설치하면 된다.

```sh
$ vue init nuxt-community/starter-template <project-name>
$ cd <project-name>
$ npm i
```

<br/>

## offline 설치 방법

<br/>

간혹 당신이 속한 소속에서 보안상 문제로 해당 사이트나 커맨드를 차단하여 nuxt-community에 속한 tempate을 vue-cli로 설치가 되지 안는 경우가 있다.
이런 경우에는 `offline`으로 설치가 가능하다.

우선 사용자 경로에 **.vue-templates** 폴더를 하나 생성하자.

```sh
C:\Users\사용자이름\.vue-templates
```

그다음 [Starter-Template](https://github.com/nuxt-community/starter-template)의 Github에서 해당 소스를 clone을 받고 위에서 생성한 폴더로 옮겨놓자.

```sh
$ git clone https://github.com/nuxt-community/starter-template
```

이 상태로 우리는 vue-cli를 통해서 github가 아닌 로컬에 저장된 템플릿을 사용할 수 있다.

```sh
$ vue init starter-template <project-name> --offline
```

<br/>

## Nuxt.js 단일 설치

<br/>

Nuxt.js 단독으로 설치할 경우엔 아래와 같이 명령어를 실행한다.

```sh
$ npm i nuxt
```

<br/>

## Nuxt.js 디렉토리 구조

<br/>

Nuxt.js의 기본 템플릿을 사용하였다면 이미 스캐폴딩이 되어 있을 것이다. 
만약 단일로 Nuxt.js를 설치하였다 하더라도 해당 섹션을 보아야 하는 이유는 Nuxt.js 자체 내에서 필수로 있어야 하는 폴더들이 있기 때문이다.

디렉토리 구조를 파악해 보자.

<br/>

### assets

css, image, font와 같은 리소스들을 포함한다.

### components

애플리케이션에서 사용될 컴포넌트들을 포함하며 해당 경로에 위치된 컴포넌트들은 Nuxt.js의 비동기 데이터 함수인 
`asyncData`또는 `fetch`를 사용할 수 없다.

### layouts

애플리케이션 전체에 대한 레이아웃을 포함한다. 
기본으로 default.vue가 생성되어 있을 것이고 상황에 맞게 layout을 생성할 수 있다. 
`해당 디렉토리는 이름을 변경할 수 없다.`

### middleware

애플리케이션에서 사용될 middleware를 포함한다. 
middleware는 페이지 또는 레이아웃이 렌더링 되기 전에 실행이 되며, middleware를 페이지나 레이아웃에 바인딩하였다면 
`해당 페이지나 레이아웃이 실행되기 전에 매번 실행된다.`

### pages

실제 애플리케이션의 페이지 구성을 포함하며 해당 디렉토리의 구조에 따라 router가 자동으로 생성된다. 
`해당 디렉토리는 이름을 변경할 수 없다.`

### plugins

애플리케이션에 바인딩 될 외부 혹은 내부 plugins를 포함한다.
plugins는 애플리케이션이 인스턴스 화 되기 전에 실행하며 전역적으로 구성 요소를 등록하고 함수 또는 상수를 삽입할 수 있다.

### static

해당 디렉토리는 정적인 파일들을 포함한다. 
구성에 따라서 html, Javascript 파일도 포함 시킬 수 있다. 
`해당 디렉토리는 이름을 변경할 수 없다.`

### store

애플리케이션에서 사용될 vuex store 파일들을 포함한다. 
기본적으로 비활성화 상태이고 store 디렉토리에 index.js 파일을 작성하면 store가 활성화된다. 
구성에 따라서 모듈 형태의 store를 형성할 수 있다. 
`해당 디렉토리는 이름을 변경할 수 없다.`

<br/>

## Nuxt.js 실행

<br/>

최초 템플릿을 설치하면 커맨드는 4가지가 존재한다.
