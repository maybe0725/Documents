# MobX (2) 리액트 프로젝트에서 MobX 사용하기

<br/>

MobX 는 리액트 종속적이진 않지만, 리액트에서 쓰려고 만들어졌기 때문에 함께 사용하면 엄청난 시너지가 발생합니다.
더 쉬운 글로벌 상태 관리는 물론이고, setState 도 쓸 필요가 없게 됩니다.

<br/>

### 2-1. MobX 가 리액트를 만나면

우리가 이전 섹션에서 decorator 문법을 통해서 더 편하게 MobX 를 사용하는 방법을 배웠었는데요, 우리가 만약에 create-react-app 으로 프로젝트를 만들면 기본적으로는 decorator 를 사용하지 못하기 때문에 따로 babel 설정을 해줘야 합니다.

우선, decorator 없이 리액트에서 MobX 를 사용하는 방법을 알아볼게요!

<br/>

### 프로젝트 준비하기

다음 명령어를 통해 평범한 리액트 프로젝트를 만들어주세요.

```
[use yarn]
$ npx create-react-app mobx-with-react
$ cd mobx-with-react
$ yarn add mobx mobx-react
$ yarn start

[use npm]
$ npx create-react-app mobx-with-react
$ cd mobx-with-react
$ npm install mobx --save
$ npm install mobx-react --save
$ npm start
```

<br/>

### 카운터 만들기

MobX 를 사용해서 엄청나게 간단한 카운터를 만들어보겠습니다.

`src/Counter.js`

```javascript
import React, { Component } from "react";
import { decorate, observable, action } from "mobx";
import { observer } from "mobx-react";

class Counter extends Component {
  number = 0;

  increase = () => {
    this.number++;
  };

  decrease = () => {
    this.number--;
  };

  render() {
    return (
      <div>
        <h1>{this.number}</h1>
        <button onClick={this.increase}>+1</button>
        <button onClick={this.decrease}>-1</button>
      </div>
    );
  }
}

decorate(Counter, {
  number: observable,
  increase: action,
  decrease: action
});

export default observer(Counter);
```

다 만드셨으면 App.js 에 반영해주세요.

`src/App.js`

```javascript
import React, { Component } from "react";
import Counter from "./Counter";

class App extends Component {
  render() {
    return (
      <div>
        <Counter />
      </div>
    );
  }
}

export default App;
```

<p><a target="_blank" href="https://codesandbox.io/s/o46655y855"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit mobx-with-react"></a></p>

카운터가 잘 작동하는지 확인해보세요.

진짜 간단하죠..? 뭔가 리액트스럽지가 않습니다. setState 없이 그냥 값 바꿔주면 알아서 렌더링해줍니다. 어떻게 알아서 렌더링해주냐구요? 코드 최하단에서 사용된 observer 가 observable 값이 변할 때 컴포넌트의 forceUpdate 를 호출하게 함으로써 자동으로 변화가 화면에 반영됩니다.

이게 성능상으로 과연 좋을까 걱정이 될 수도 있긴 하지만 놀랍게도 최적화가 많이 되어있어서 그 부분에 대해서는 크게 걱정하실 필요없습니다.

이런식으로, 리액트에서 MobX 를 사용할 땐 리덕스에서 했던 것 처럼 따로 다른 파일로 스토어를 만들 필요도 없고 (필요하면 만들 수도 있습니다) 그냥 컴포넌트에 바로 적용해줄 수 있습니다.

<br/>

### Decorator 와 함께 사용하기

여기서, decorator 를 사용하면 훨씬 더 편하게 문법을 작성 할 수 있는데요, 그러려면 babel 설정을 해주셔야 합니다. babel 설정을 커스터마이징 하려면 `yarn eject` 를 해야합니다.

> 설정방법 1.

```
[yarn]
$ yarn eject
$ yarn add @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators

[npm]
$ npm run eject
$ npm install @babel/plugin-proposal-class-properties @babel/plugin-proposal-decorators
```

그리고 나서, package.json 을 열으신 다음에, babel 쪽을 찾아서 다음과 같이 수정해주세요.

```json
 "babel": {
    "presets": [
      "react-app"
    ],
    "plugins": [
        ["@babel/plugin-proposal-decorators", { "legacy": true}],
        ["@babel/plugin-proposal-class-properties", { "loose": true}]
    ]
  }
```

위에 설정 방법외에 다른 설정 방법을 추가 합니다.

> 설정방법 2.

```
[yarn]
$ yarn eject
$ yarn add babel-preset-mobx

[npm]
$ npm run eject
$ npm install babel-preset-mobx
```

`package.json`

```
{
  "앞부분은": "생략",
  "babel": {
    "presets": [
      "react-app",
      "mobx"
    ]
  },
  "이후 코드도": "생략"
}

// 순서가 중요합니다. mobx가 react-app보다 뒤에 있어야 합니다.
```

설정을 다 하셨으면, 개발서버를 껐다 켜주시고, Counter 코드를 decorator 로 더 깔끔하게 작성해보겠습니다.

<br/>

`src/Counter.js`

```javascript
import React, { Component } from "react";
import { observable, action } from "mobx";
import { observer } from "mobx-react";

// **** 최하단에 잇던 observer 가 이렇게 위로 올라옵니다.
@observer
class Counter extends Component {
  @observable number = 0;

  @action
  increase = () => {
    this.number++;
  };

  @action
  decrease = () => {
    this.number--;
  };

  render() {
    return (
      <div>
        <h1>{this.number}</h1>
        <button onClick={this.increase}>+1</button>
        <button onClick={this.decrease}>-1</button>
      </div>
    );
  }
}

// **** decorate 는 더 이상 필요 없어집니다.
// decorate(Counter, {
//   number: observable,
//   increase: action,
//   decrease: action
// })

// export default observer(Counter);
// **** observer 는 코드의 상단으로 올라갑니다.
export default Counter;
```

<p><a target="_blank" href="https://codesandbox.io/s/zq52xy34z4"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit mobx-with-react"></a></p>

어떤가요? decorator 문법을 사용하니까 조금 더 깔끔하지 않나요? 저는 그렇다고 생각하는데, 취향에 따라 싫을 수 도 있습니다. 우선 우리가 이 튜토리얼의 상단부에서 다뤘던것처럼 decorator 사용은 필수는 아니라는 점, 알아두세요!

<br/>

### 2-2. MobX 스토어 분리시키기

MobX 에도 리덕스처럼 스토어라는 개념이 있습니다. 리덕스는 하나의 앱에는 단 하나의 스토어만 있지만, MobX 에서는 여러개를 만들어도 됩니다. 이번에는, 한번 카운터의 상태 관련 로직을 스토어로 따로 분리시키는 작업을 해보도록 하겠습니다.

<br/>

### 스토어 만들기

MobX 에서 스토어를 만드는건 생각보다 간단합니다. 리덕스처럼 리듀서나, 액션 생성함수.. 그런건 없습니다. 그냥 하나의 클래스에 observable 값이랑 함수들을 만들어주면 끝!

`stores/counter.js`

```javascript
import { observable, action } from "mobx";

export default class CounterStore {
  @observable number = 0;

  @action increase = () => {
    this.number++;
  };

  @action decrease = () => {
    this.number--;
  };
}
```

정말 간단하지요?

<br/>

### Provider 로 프로젝트에 스토어 적용

MobX에서 프로젝트에 스토어를 적용 할 때는, Redux 처럼 Provider 라는 컴포넌트를 사용합니다. 프로젝트의 엔트리 파일인 index.js 파일을 다음과 같이 수정해보세요.

`src/index.js`

```javascript
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "mobx-react"; // MobX 에서 사용하는 Provider
import "./index.css";
import App from "./App";
import registerServiceWorker from "./registerServiceWorker";
import CounterStore from "./stores/counter"; // 방금 만든 스토어 불러와줍니다.

const counter = new CounterStore(); // 스토어 인스턴스를 만들고

ReactDOM.render(
  <Provider counter={counter}>
    {/* Provider 에 props 로 넣어줍니다. */}
    <App />
  </Provider>,
  document.getElementById("root")
);

registerServiceWorker();
```

<br/>

### inject 로 컴포넌트에 스토어 주입

[inject](https://github.com/mobxjs/mobx-react#provider-and-inject) 함수는 mobx-react 에 있는 함수로서, 컴포넌트에서 스토어에 접근할 수 있게 해줍니다. 정확히는, 스토어에 있는 값을 컴포넌트의 props 로 "주입"을 해줍니다.

Counter.js 를 다음과 같이 수정해보세요.

`src/Counter.js`

```javascript
import React, { Component } from "react";
import { observer, inject } from "mobx-react";

@inject("counter")
@observer
class Counter extends Component {
  render() {
    const { counter } = this.props;
    return (
      <div>
        <h1>{counter.number}</h1>
        <button onClick={counter.increase}>+1</button>
        <button onClick={counter.decrease}>-1</button>
      </div>
    );
  }
}

export default Counter;
```

<p><a target="_blank" href="https://codesandbox.io/s/v3j77p1o05"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit mobx-with-react"></a></p>

위와 같이 `inject('스토어이름')` 을 하시면 컴포넌트에서 해당 스토어를 props 로 전달받아서 사용 할 수 있게 됩니다.

만약에, 마치 리덕스에서의 mapStateToProps / mapDispatchToProps 처럼 스토어의 특정 값이나 함수만 넣어주고 싶다면 이렇게 하실 수도 있습니다.

`src/Counter.js`

```javascript
import React, { Component } from "react";
import { observer, inject } from "mobx-react";

// **** 함수형태로 파라미터를 전달해주면 특정 값만 받아올 수 있음.
@inject(stores => ({
  number: stores.counter.number,
  increase: stores.counter.increase,
  decrease: stores.counter.decrease
}))
@observer
class Counter extends Component {
  render() {
    const { number, increase, decrease } = this.props;
    return (
      <div>
        <h1>{number}</h1>
        <button onClick={increase}>+1</button>
        <button onClick={decrease}>-1</button>
      </div>
    );
  }
}

export default Counter;
```

<p><a target="_blank" href="https://codesandbox.io/s/1ormzmwjvq"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit mobx-with-react"></a></p>

이제 컴포넌트는, 유저 인터페이스와, 인터랙션만 관리하면 되고 상태 관련 로직은 모두 스토어로 분리되었습니다.

리덕스에서는, 우리가 프리젠테이셔널 컴포넌트 / 컨테이너 컴포넌트 라는 개념에 대해서 알아보았었습니다. 단순히 props 값을 가져오기만 해서 받아오는 컴포넌트는 프리젠테이셔널 컴포넌트라고 부르고, 스토어에서 부터 값이나 액션 생성함수를 받아오는 컴포넌트를 컨테이너 컴포넌트라고 부른다고 했었죠.

리덕스 진영에서는, 문서에서도 그렇고 생태계 쪽에서도 그렇고 프리젠테이셔널 / 컨테이너로 분리해서 작성하는게 일반적입니다. 반면, MobX 에서는, 딱히 그런걸 명시하지 않습니다. 그래서, 굳이 번거롭게 컨테이너를 강제적으로 만드실 필요는 없습니다. 하지만, 하셔도 무방합니다!

<br/>
<br/>

> 참조 - 퍼온글
>
> - [MobX (2) 리액트 프로젝트에서 MobX 사용하기](https://velog.io/@velopert/MobX-2-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8%EC%97%90%EC%84%9C-MobX-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-oejltas52z)
