# Vue.js Router Example

<br/>

## 1. Create Vue CLI Project

```sh
### yarn 설치
$ npm install -g yarn

### vue-cli 설치
[use npm]
$ npm install -g @vue/cli

[use yarn]
$ yarn global add @vue/cli

### create vue cli project
$ vue create <project-name>

  Vue CLI v3.10.0
  ? Please pick a preset: Manually select features
  ? Check the features needed for your project: Babel, Router, Vuex, Linter
  ? Use history mode for router? (Requires proper server setup for index fallback in oduction) Yes
  ? Pick a linter / formatter config: Prettier
  ? Pick additional lint features: (Press <space> to select, <a> to toggle all, <i> to vert selection)Lint on save
  ? Where do you prefer placing config for Babel, PostCSS, ESLint, etc.? In package.json
  ? Save this as a preset for future projects? No

$ cd <project-name>
$ npm run serve
```

Vue CLI Manually 생성 시 각 항목에 대한 설명은 다음과 같다.

- `Babel` : ES6(ES2015) 이상 버전이나 typescript로 코딩을 한경우 범용적인 ES5버전으로 자동 전환 해줌
- `TypeScript` : 앵귤러js에서 표준으로 삼고있는 코딩언어로 javascript에 타입을 강화한 좀 더 진화된 javascript라고 보면됨
- `Progressive Web App Support` : 웹앱을 만들고자 한다면 선택
- `Router` : Vue에서 화면이동을 구현하기 위한 플러그인
- `Vuex` : Vue에서 데이터를 쉽게 공유해서 대규모 Vue 개발을 편리하게 해줌
- `CSS Pre-processors` : SASS, LESS같이 화면을 꾸며주는 CSS를 프로그램처럼 작업할 수 있게 해줌
- `Linter/Formatter` : js코딩을 할 때 대부분의 사람들이 쉽게 알아 볼 수 있게 표준 가이드를 해줌
- `Unit Testing` : just, 모카 등 단위테스트를 자동으로 해 줄수 있는 플러그인
- `E2E Testing` : E2E(End-to-End) 테스트로 통합테스트 정도로 생각하면 됨

<br/>

## 2. Add vuetify Plugin

```sh
### Add Plugin
$ vue add <Plugin>

### Add vuetify Plugin
$ vue add vuetify

  �  Installing vue-cli-plugin-vuetify...

  + vue-cli-plugin-vuetify@0.6.3
  added 1 package from 1 contributor and audited 24202 packages in 12.013s
  found 0 vulnerabilities

  ✔  Successfully installed plugin: vue-cli-plugin-vuetify

  ? Choose a preset: Configure (advanced)
  ? Use a pre-made template? (will replace App.vue and HelloWorld.vue) Yes
  ? Use custom theme? No
  ? Use custom properties (CSS variables)? No
  ? Select icon font Font Awesome 5
  ? Use fonts as a dependency (for Electron or offline)? No
  ? Use a-la-carte components? No
  ? Use babel/polyfill? Yes
  ? Select locale English
```

<br/>

## FontAwesome 설치.

`무료 버전의 FontAwesome 설치.`

```sh
# yarn use
$ yarn add @fortawesome/fontawesome-free -D

# npm use
$ npm install @fortawesome/fontawesome-free -D
```

`index.html 파일 FontAwesome CDN 확인`

```html
<link
  href="https://use.fontawesome.com/releases/v5.0.13/css/all.css"
  rel="stylesheet"
/>
```

`vuetify.js 확인`

```javascript
import "@fortawesome/fontawesome-free/css/all.css"; // Ensure you are using css-loader
import Vue from "vue";
import Vuetify from "vuetify/lib";

Vue.use(Vuetify);

export default new Vuetify({
  icons: {
    iconfont: "fa"
  }
});
```

> 참고
- https://vuetifyjs.com/ko/components/icons
