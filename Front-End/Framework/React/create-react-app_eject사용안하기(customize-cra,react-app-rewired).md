# create-react-app에서 eject사용안하기<br/>(customize-cra, react-app-rewired)

<br/>

## CRA를 쓰는 이유

페이스북에서 개발한 Create-React-App(CRA) 보일러 플레이트를 사용하게 되면 간편하게 리액트 프로젝트를 시작할 수 있다. CRA는 리액트 프로젝트를 처음 실행할 때 필요한 여러가지 라이브러리나 웹팩의 설정 없이 간편하게 프로젝트를 시작할 수 있다. CRA가 가지는 장점 중 몇가를 나열해보겠다.

1. 단 하나의 one build dependency를 가지게 되므로 React프로젝트를 구성할 때 필요한 Webpack, Babel, ESLint 등 간의 연결에 대해서 신경쓰지 않아도 된다.
2. 위에서 나열한 Webpack, Babel, ESLint는 처음 프로젝트를 구성할 때 반드시 설정이 필요한 패키지들이다. 이를 처음 설정할때 상당한 시간이 소요될 수 있는데, CRA는 프로젝트에 필요한 필수적인 설정(Configuration)을 대신 해준다.
3. CRA는 Autoprefixer를 지원해준다. 즉, 일반적인 CSS코드 생성을 하게 되면 자동으로 -webkit-, -ms- 등을 자동으로 적용해준다.

그 외에 CRA의 특징들은 https://github.com/facebook/create-react-app 주소를 참조해주길 바란다.

<br/>

## eject를 쓰는 이유?

eject는 one build dependency를 가진 프로젝트를 custom하기 위한 방법이다. 단순히 CRA프로젝트에서 다음의 명령어를 수행하면 된다.

```
[use yarn]
  $ yarn eject

[use npm]
  $ npm run eject
```

숨겨져 있던 모든 설정들(All configurations, webpack설정이나 babel설정 파일 등…)와 패키지들이 가지는 의존성(dependencies)을 볼 수 있게 된다.
주의할 점은, 한번 eject를 수행하게 되면 이전 상태로 되돌아 갈 수 없다.

<br/>

## eject를 쓰면 안되는 이유

eject는 우리의 프로젝트를 자유롭게 customizing 한다는 점에서 매력적으로 보인다. 하지만 몇가지 CRA를 쓰면서 갖게 되는 이점을 포기하게 된다.

1. CRA의 모든 configuration 을 직접 유지보수 해야 된다. Webpack, Babel 등등… 이 외에도 프로젝트를 진행하게 되면, 컴포넌트, 테스트 등 많은 작업을 해야 하는데…Webpack, Babel 설정까지…(eject를 쓰는 개발자들은 존경스럽다…). 게다가 협업을 하는 상황이라면, 같이 일하는 개발자들도 같이 Webpack과 Babel에 상당히 익숙해져야 한다.
2. One Build Dependency의 장점을 잃게 된다. 작업 도중 하나의 패키지가 필요해서 설치한다거나, 삭제할 때, 항상 다른 패키지들과의 의존성을 신경써야 한다.

그럼에도 불구하고 eject를 하고 싶다면, 몇가지 고려해봐야 할 것이 있다.

1. 얼마나 많은 설정을 기존 CRA 설정에 추가하거나 뺄 것인가?
2. 추가하고 싶은 설정의 중요성이 eject를 하기 이전에 가지는 장점보다 큰가?

<br/>

## react-app-rewired, customize-cra 소개

반드시 eject하지 않아도 기존 CRA프로젝트에 약간의 customizing을 할 수 있는 방법이 있다.(물론, eject하는것보다 자유롭진 않다.)

`customize-cra`와 `react-app-rewired`는 나의 CRA프로젝트를 eject하지 않고 customizing할 수 있게 도와주는 라이브러리이다. 현재 CRA2 버전을 사용하고 있다면 customize-cra와 react-app-rewired를 같이 사용하고, CRA1버전을 사용하고 있다면 react-app-rewired만을 사용하면 된다.

주의할 것은, 이것들 역시 절대적으로 CRA가 가지는 안정성을 보장해주지 않는다는 것이다! 다음 링크에서 React개발자 Dan Abramov가 이에 대해 언급할 것을 찾아볼 수 있다.

[Dan Abramov](https://github.com/facebook/create-react-app/issues/99?source=post_page---------------------------)

<br/>

## Customize-CRA 로 Decorator 설정 추가해보기

간단히 소개를 하였으니, 이제 실제로 webpack설정에 es7문법 중 하나인 decorator를 사용하기 위한 설정을 해보도록 하겠다. 진행할 CRA버전은 2로 하도록 하겠다. https://github.com/facebook/create-react-app 링크를 참조하여 create-react-app 프로젝트를 생성해 보도록 하겠다.

```
[use yarn]
  $ yarn create react-app cra_not_eject
  $ cd cra_not_eject
  $ yarn add customize-cra --dev
  $ yarn add react-app-rewired --dev

[use npm]
  $ npx react-create-app cra_not_eject
  $ cd cra_not_eject
  $ npm install customize-cra --save-dev
  $ npm install react-app-rewired --save-dev
```

이제, package.json에서 script를 수정하여 프로젝트 실행시 react-scripts 대신 react-app-rewired를 통해 실행하도록 하자.

`package.json`

```json
  "scripts": {
-   "start": "react-scripts start",
-   "build": "react-scripts build",
-   "test" : "react-scripts test",
+   "start": "react-app-rewired start",
+   "build": "react-app-rewired build",
+   "test" : "react-app-rewired test",
    "eject": "react-scripts eject"
  },
```

마지막으로, 프로젝트 가장 최상단 위치에서 `config-overrides.js` 라는 파일을 생성한 뒤, 원하는 customizing을 하면 된다. config-overrides.js 를 작성하기 전에, decorator 문법을 사용하기 위해 필요한 패키지를 설치하자.

```
[use yarn]
  $ yarn add --dev @babel/plugin-proposal-decorators

[use npm]
  $ npm install --save-dev @babel/plugin-proposal-decorators
```

`config-overrides.js`

```javascript
const {
  override,
  addDecoratorsLegacy, // customize-cra가 지원해주는 addDecoratorsLegacy 를 통해 decorator문법을 사용할 수 있게 되었다.
  disableEsLint // mobx-react의 observer decorator를 App.js에 적용해 볼 것이다. customize-cra 문서에서 disableEsLint 플러그인을 같이 사용하라고 나타나있기 때문에 설정파일에 명시하였다.
} = require("customize-cra");

module.exports = {
  webpack: override(disableEsLint(), addDecoratorsLegacy())
};
```

이제 우리는 eject없이 decorator문법을 사용할 수 있게 되었다. 실제 블로그에서 다루는 주요한 내용은 끝났다. decorator 사용을 아는 사람이라면 하단 내용은 스킵하여도 괜찮다.

이제 실제로 간단하게 decorator를 App.js에 적용해 보도록 하겠다.

<br/>

## App.js 파일에 mobx observer decorator 적용

다른 js파일을 생성하여 decorator문법을 적용하기에는 본 블로그 내용이 너무 길어지므로, 간단하게 App.js 파일을 class로 변경한 뒤 observer decorator를 적용해보도록 하겠다. 우선 mobx, mobx-react를 설치한 뒤, decorator를 적용해 보도록 하자.

```
[use yarn]
  $ yarn add mobx mobx-react

[use npm]
  $ npm install mobx --save
  $ npm install mobx-react --save
```

`App.js`

```javascript
import React from "react";
import logo from "./logo.svg";
import "./App.css";
import { observer } from "mobx-react";

@observer
export default class App extends React.Component {
  render() {
    return (
      <div className="App">
        <header className="App-header">
          <img src={logo} className="App-logo" alt="logo" />
          <p>
            Edit <code>src/App.js</code> and save to reload.
          </p>
          <a
            className="App-link"
            href="https://reactjs.org"
            target="_blank"
            rel="noopener noreferrer"
          >
            Learn React
          </a>
        </header>
      </div>
    );
  }
}
```

class형태의 코드에만 decorator를 적용할 수 있기에 App.js를 함수형 컴포넌트에서 클래스 컴포넌트로 변경하였다. 이제 yarn start를 실행해보면, 별다른 에러없이 프로젝트가 실행되는 것을 볼 수 있다.

이렇게 간단하게 eject없이 cra 프로젝트를 customizing 해보았다.

<br/><br/>

> Reference - 퍼온글
>
> - [Create-React-App에서 Eject사용안하기(customize-cra, react-app-rewired)](https://medium.com/@jsh901220/create-react-app%EC%97%90%EC%84%9C-eject%EC%82%AC%EC%9A%A9%EC%95%88%ED%95%98%EA%B8%B0-customize-cra-react-app-rewired-10a83522ace0)
