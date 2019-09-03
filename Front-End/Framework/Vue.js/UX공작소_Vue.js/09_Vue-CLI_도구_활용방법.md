# Vue-CLI 도구 활용방법

<br/>

원본글 출처

- [Vue-CLI 도구 활용방법](https://ux.stories.pe.kr/136)

<br/>

Vue를 활용하여 개발 한다면 `Vue-CLI`는 반드시 알고 있어야 하는 도구 입니다.

`Vue-CLI`가 Vue의 코어는 아니지만 개발자가 쉽게 Vue 프로젝트를 개발 할 수 있게 해주는 아주 유용한 도구입니다.

<br/>

## Vue-CLI 란 무엇인가???

<br/>

`Vue-CLI`에서 CLI는 `Command Line Interface`의 약자로 윈도우에서는 `Command 창`, 맥에서는 `터미널 창`에서 타이핑으로 명령어를 입력하여 원하는 바를 실행 시키는 도구를 말합니다.

Vue-CLI은 내부적으로 `Webpack`을 활용합니다. Vue-cli로 명령을 실행 시키면 cli가 자동으로 최적화된 `Webpack` 형태의 결과물을 생성 시켜 줍니다.

<br/>

## Vue-CLI로 할 수 있는 것은 무엇인가???

<br/>

그럼 Vue-cli로 할 수 있는 것이 무엇이냐???

- `새로운 Vue 프로젝트 생성` : 몇가지 스케폴딩(기본 골격)을 선택하여 Vue 프로젝트를 빠르게 생성할 수 있습니다.
- `Vue 플러그인 설치/삭제` : 다양한 vue플러그인들을 추가하거나 삭제할 수 있습니다.
- `vue.config.js 설정` : 웹팩의 구성에 대해 vue.config.js로 오버라이딩하여 추가 설정할 수 있습니다.
- `Vue GUI 도구 사용` : cli가 낮선 개발자를 위해 GUI형태로도 도구를 제공해 줍니다.

<br/>

## Vue-CLI 설치하기

<br/>

Vue-CLI를 사용하기 위해서는 먼저 `node.js`가 설치되어 있어야 하고 node.js에 속해 있는 npm을 통해 vue-cli 패키지를 설치해야 사용할 수 있습니다. 이때 Vue-cli를 전역으로 설치한다면 컴퓨터의 어디에서나 사용이 가능하기 때문에 전역으로 설치 할 것을 권장합니다.

`node.js`가 설치되어 었다는 가정 하에 아래의 npm명령어를 실행시키면 됩니다.

<br/>

### Windows에서 전역으로 설치

```sh
npm install -g @vue/cli
```

<br/>

### MacOS에서 전역으로 설치

```sh
sudo npm install -g @vue/cli
```

만약 전역으로 설치하지 않고 해당 프로젝트에만 설치(로컬)하기를 원한다면 명령어 중에 -g를 빼고 실행 시키면 됩니다.

<br/>

## Vue-CLI 명령어

<br/>

### 프로젝트 생성하기

Vue-cli로 프로젝트를 생성하는 방법입니다.

vue-cli를 설치하면 vue라는 명령어를 사용할 수 있게 됩니다.

그중에 create명령어 뒤에 프로젝트명을 입력하면 해당 이름으로 vue 프로젝트가 스케폴딩 됩니다.

```sh
vue create <프로젝트명>
```

해당 명령어를 실행시키면 컴퓨터가 몇가지 질문을 하고 그에 해당하는 답변을 하면 결과물이 생성됩니다.

![images](https://t1.daumcdn.net/cfile/tistory/9981FD415C9CE26B29)

babel과 eslint가 포함되어 있는 Default(기본)로 선택할지, 아니면 내가 원하는 것을 직접 선택(Manually)할 지에 대한 선택지가 주어집니다.

> 선택은 키보드의 위아래 화살표로 이동해서 Enter키를 눌러 실행합니다.

Default(기본)를 선택했다면 바로 설치가 진행됩니다. 더 선택의 여지는 없습니다.

![images](https://t1.daumcdn.net/cfile/tistory/99D1B7495C9CE26B16)

만약 Manually select features를 선택한다면 설치될 플러그인을 선택할 수 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/9946D24A5C9CE26B20)

Vue-cli의 버전에 따라 다르겠지만 현재 시점인 v3.0.1일때의 경우 9가지의 선택지가 주어집니다.

> 복수 선택이 가능합니다. <br/>
> 선택은 키보드의 위아래 화살표로 이동해서 스페이스바를 눌러 추가 선택을 하고<br/>
> 최종적으로 선택이 모두 되었으면 Enter키를 눌러 실행시킵니다.

각각의 요소에 대한 설명은 아래를 참조하세요.

- `Babel` : ES6(ES2015) 이상 버전이나 typescript로 코딩을 한경우 범용적인 ES5버전으로 자동 전환 해줌
- `TypeScript` : 앵귤러js에서 표준으로 삼고있는 코딩언어로 javascript에 타입을 강화한 좀 더 진화된 javascript라고 보면됨
- Progressive Web App Support : 웹앱을 만들고자 한다면 선택
- Router : Vue에서 화면이동을 구현하기 위한 플러그인
- Vuex : Vue에서 데이터를 쉽게 공유해서 대규모 Vue 개발을 편리하게 해줌
- CSS Pre-processors : SASS, LESS같이 화면을 꾸며주는 CSS를 프로그램처럼 작업할 수 있게 해줌
- Linter/Formatter : js코딩을 할 때 대부분의 사람들이 쉽게 알아 볼 수 있게 표준 가이드를 해줌
- Unit Testing : just, 모카 등 단위테스트를 자동으로 해 줄수 있는 플러그인
- E2E Testing : E2E(End-to-End) 테스트로 통합테스트 정도로 생각하면 됨

선택한 설치요소에 따라 각각 또 세부적인 선택을 물어 봅니다.

![imgages](https://t1.daumcdn.net/cfile/tistory/99F6D7355C9CE26B18)

▲ 라우터 설정 중에 히스토리 모드를 사용할 것이냐고 묻는다. 보통은 기본값인 `Y`를 선택합니다.

![images](https://t1.daumcdn.net/cfile/tistory/9926F3465C9CE26B1F)

▲ CSS 프리프로세서를 무엇으로 선택할 것인가를 선택합니다.
본인이 알고 있는 것을 선택해서 사용하면 됩니다.

이게 무엇인지 모른다면 처음부터 프리프로세서를 선택 하지 않는 것이 좋습니다.

![images](https://t1.daumcdn.net/cfile/tistory/992DA6345C9CE26B30)

▲ ESlinter를 적용하는데 어느 규칙으로 사용할 것인가를 선택합니다.

Standard는 기본규칙이고 Airbnb는 숙박공유회사인 Airbnb에서 자기내 회사에서 사용하는 코딩규칙을 공개 했고 그 규칙을 사용할 수도 있습니다.

![images](https://t1.daumcdn.net/cfile/tistory/9919BE3D5C9CE26B17)

▲ 단위테스트로 무엇을 사용할 것인가를 선택합니다.

현재 Jest를 많이 사용하는 추세라고 합니다.

![images](https://t1.daumcdn.net/cfile/tistory/99B1F04C5C9CE26B19)

▲ 지금까지 선택한 요소들 중 일부는 설정 항목을 남기게 되어 있는데, 이 것을 별도의 파일로 만들어서 작성을 할 것이냐 아니면 기존에 있는 package.json파일에 추가 요소로 넣어서 작성할 것이냐를 선택합니다. 이것은 선택의 문제일 뿐 아무거나 선택해도 됩니다.

![images](https://t1.daumcdn.net/cfile/tistory/99BB3B3F5C9CE26B0D)

▲ 지금까지 선택한 것들을 모두 기록해 둘 것이냐를 선택합니다.

만약 Yes 를 한다면 지금까지 선택한 모든 설정을 Vue-cli에 기록으로 남겨 놓게 됩니다. 그래서 다음 번에 신규로 프로젝트를 만들 때 해당 항목도 선택요소로 보이게 되고 그것을 선택하면 자동으로 지금까지 선택한 것과 동일한 환경을 세팅해 줍니다.

이렇게 선택을 하면 자동으로 설치가 진행이 됩니다.

설치가 완료되면 아래와같은 폴더가 생성됩니다.

- node_modules : 모든 nodejs의 프로젝트가 공통으로 가지는 폴더로 실제 설치된 모듈이 저장되는 폴더입니다. (손델 필요 없는 폴더 입니다.)
- public : 프로젝트를 배포할 때 필요한 파일들이 들어 있습니다. 특별한 경우를 빼고는 이것도 건드릴 필요는 없습니다.
- src : 소스 폴더를 저장하는 폴더로 이 폴더 안에다가 개발 코딩을 하는 핵심폴더입니다.
- tests : jest같은 단위테스트를 하기 위한 폴더입니다.
- package.json : 모든 nodejs의 프로젝트가 공통으로 가지는 설정 저장용 파일입니다.
- babel.config.js : js의 버전을 변환해 주는 babel의 설정파일 입니다.
- yarn.lock : 근래 npm보다 많이 사용하는 패키지관리시스템의 lock파일입니다. 그냥 놔두시면 됩니다. 건들필요 없습니다.
- dist : 프로젝트를 빌드하거나 컴파일하면 생성되는 폴더이며 최종 결과물이라고 보시면 됩니다.

<br/>

### 프로젝트에 플러그인 추가하기

<br/>

생성된 프로젝트에 추가로 플러그인을 설치하는 명령어 입니다.

이렇게 플러그인을 추가하면 자동으로 `package.json`에 기록이 남게 됩니다.

```sh
vue add <플러그인>
```

<br/>

## Vue CLI GUI 화면

<br/>

Vue-cli가 이름은 Command화면에서 타이핑으로 명령어를 실행하는 뉘앙스이지만 사용자의 편리를 위해 GUI화면도 제공하고 있습니다.
간단하게 해당 프로젝트 폴더가 있는 Command창에서 vue ui 명령어만 치면 됩니다.

```sh
vue ui
```

![images](https://t1.daumcdn.net/cfile/tistory/99054F375C9CE26B07)

vue ui 명령어를 실행하면 위와 같이 브라우저로 GUI화면이 띄워집니다.

이화면에서 vue create와 같이 프로젝트를 생성할 수도 있고 플러그인을 추가할 수도 있습니다.

초보자 뿐만 아니라 숙달자도 쉽게 사용할 수 있게 기능을 제공하고 있습니다.
