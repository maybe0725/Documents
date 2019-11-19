# Redux (2) 리액트 없이 쓰는 리덕스

<br/>

리덕스는 리액트에 종속되지 않습니다. 리액트에서 사용하려고 만든거긴 하지만, 실제로 다른 UI 라이브러리나 프레임워크와 함께 사용 될 수도 있습니다 (예: angular-redux, ember-redux...) 물론, 바닐라 자바스크립트와도 함께 사용할 수도 있겠죠.

> 바닐라 (vanilla) 자바스크립트는, 라이브러리나 프레임워크 없이 사용하는 자바스크립트 그 자체를 의미합니다. 즉, 순수 자바스크립트라고 이해하시면 됩니다.

한번, 바닐라 자바스크립트를 사용하여 스위치와 카운터를 만들어보면서 리덕스를 학습해봅시다.

다음 링크를 클릭해주세요:

<p><a target="_blank" href="https://codesandbox.io/s/pj9jonlxxm"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS Redux Boilerplate"></a></p>

위 프로젝트에서는 다음과 같이 스위치와, 카운터의 HTML/CSS 만 완성되어있고, index.js 에서는 리덕스의 createStore 라는 함수만 불러와져있는 상태입니다.

여기에서, 한번 리덕스를 사용해보겠습니다.

<br/>

### DOM 레퍼런스 가져오기

우린 이번엔 따로 UI 라이브러리를 사용하지 않기 때문에 DOM 을 직접 건들여줘야합니다.

그렇기 때문에, DOM API 들을 사용하여 HTML 상에 나타나고 있는 각 요소들에 대한 레퍼런스를 만들어주겠습니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("light")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");
```

요소들을 잘 가져왔다면 다음과 같이나타날 것입니다.

잘 나타나는것을 확인했다면 최하단의 console.log 는 지워주세요.

<br/>

### 액션 타입 정의

우선, 액션 타입을 정의하겠습니다. 프로젝트에서 상태에 변화를 일으키는것을 하나의 액션으로 보고, 그 액션에 대한 이름을 정해주는 과정입니다. 이름은 문자열 형태로, 주로 대문자로 작성하며 액션 이름은 고유해야합니다. 중복되면 안됩니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("switch")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// **** 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";
```

<br/>

### 액션 생성 함수 정의

액션 객체를 만드는 함수를, 액션 생성 함수라고 부릅니다. 액션 객체는 type 값을 필수로 들고있어야 하며, 나머지 액션에서 참고하고 싶은 값들은 개발자 마음대로 넣을 수 있습니다. (액션에 들어가는 추가적인 값을 모두 payload 라는 이름으로 설정하는 개발방식도 있습니다. 이에 대해선 나중에 알아보게됩니다.)

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("switch")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// **** 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });
```

<br/>

### 초깃값 설정

프로젝트에서 사용하는 초깃값을 정의해주겠습니다. 초깃값의 형태는 자유입니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("switch")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// **** 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};
```

초깃값에 스위치에서 사용할 light 값과, 카운터에서 사용 할 counter 를 넣어주었습니다.

<br/>

### 리듀서 함수 정의

리듀서는 변화를 일으키는 함수입니다. 파라미터로는 state 와 action 을 받아옵니다. 이 함수는 화살표함수로 작성하셔도 되고 일반형태의 함수로 작성하셔도 됩니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("switch")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};

// **** 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, // 기존의 값은 그대로 두면서
        light: !state.light // light 값 반전시키기
      };
    case INCREMENT:
      return {
        ...state,
        counter: state.counter + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      // 지원하지 않는 액션의 경우 상태 유지
      return state;
  }
}
```

리듀서 함수가 가장 처음 호출 될 때는 state 가 undefined 입니다.
state 가 undefined 로 주어졌을땐 initialState 를 사용하도록 설정하기위해서
파라미터쪽에서 기본값이 설정되어있습니다. - [기본 매개변수 문법](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Default_parameters)

리듀서에서는, 불변성을 유지해주면서 데이터에 변화를 일으켜주어야 합니다. 이러한 작업을 하기 위해서, ... spread 연산자를 사용하면 편합니다. 기존의 값을 그대로 두면서 새로운 값을 덮어쓰는 방식으로 작업하시면 됩니다. 단, 객체의 구조가 복잡해지면 (단, object.something.inside.hello.bye 처럼 객체 안의 객체 안의 객체 안의 있는 값을 바꿀때는 ... 로만 하는것은 굉장히 번거롭습니다. `리덕스의 상태는 최대한 깊지 않은 구조로 진행하는 것이 좋긴 하지만, 깊숙히 있는 값을 바꿔야 할 때 더 쉽게 바꾸는 방법에 대해서는 나중에 다뤄보게 됩니다)`

<br/>

### 스토어 만들기

이제 스토어를 만들어보겠습니다. 스토어를 만들땐 createStore 함수를 사용하며, 파라미터는 리듀서 함수를 전달해줍니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("switch")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};

// 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, // 기존의 값은 그대로 두면서
        light: !state.light // light 값 반전시키기
      };
    case INCREMENT:
      return {
        ...state,
        counter: state.counter + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      // 지원하지 않는 액션의 경우 상태 유지
      return state;
  }
}

// **** 스토어 만들기
const store = createStore(reducer);
```

여기까지! 리덕스쪽 관련 코드의 준비는 모두 끝났습니다. 이제 사용을 해주어야 할 때입니다.

<br/>

### render 함수 구현하기

render 라는 함수를 구현하겠습니다. 리액트의 render 와는 다르게, JSX 를 작성하는게 아니라 단순히 현재의 상태값에 따라 우리가 이전에 가져온 DOM 레퍼런스의 속성을 설정만 하겠습니다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("light")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};

// 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, // 기존의 값은 그대로 두면서
        light: !state.light // light 값 반전시키기
      };
    case INCREMENT:
      return {
        ...state,
        counter: state.counter + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      // 지원하지 않는 액션의 경우 상태 유지
      return state;
  }
}

// 스토어 만들기
const store = createStore(reducer);

// **** render 함수 만들기
const render = () => {
  const state = store.getState(); // 현재 상태를 가져옵니다.
  const { light, counter } = state; // 편의상 비구조화 할당
  if (light) {
    lightDiv.style.background = "green";
    switchButton.innerText = "끄기";
  } else {
    lightDiv.style.background = "gray";
    switchButton.innerText = "켜기";
  }
  counterHeadings.innerText = counter;
};

render();
```

스토어의 현재 상태를 가져올 땐 스토어의 내장함수 getState 를 사용합니다.

함수를 만들고, 이렇게 호출을 해주고 나서, 한번 초깃값을 다음과 같이 수정해보세요:

```javascript
const initialState = {
  light: true,
  counter: 713
};
```

이렇게 나타난다면 성공입니다. 결과를 확인하셨다면 다시 원상복구하세요.

<p><a target="_blank" href="https://codesandbox.io/s/vmzkzvv333"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS Redux Boilerplate"></a></p>

<br/>

### 구독 (subscribe) 하기

스토어의 상태가 바뀔 때 마다, 우리는 render 함수를 호출해줘야 합니다. 그러려먼, 우리는 스토어를 구독해주어야 합니다. 구독을 할 때에는 스토어의 내장함수 subscribe 를 사용합니다.

예시:

```javascript
const listener = () => console.log("업데이트 됐어요!");
const unsubscribe = store.subscribe(listener);
// 나중에 unsubscribe();
```

subscribe 함수의 파라미터로는, 함수형태의 값을 전달해줍니다. 이렇게 전달된 함수는, 액션이 디스패치 될 때 마다 호출이 됩니다. 그리고, subscribe 를 호출하게 되면 반환값으로 구독을 해제하는 unsubscribe 함수를 받게 되는데 나중에 필요해질 때 호출하면 됩니다.

미리 말씀드리자면, 우리는 지금 리덕스의 내부 함수들을 파악하기 위해서 리액트 없이 하기 때문에 이렇게 subscribe 함수에 대한 사용법을 익혀보고 있지만, 나중엔 우리가 리액트에서 리덕스를 쉽게 사용하기 위해 react-redux 라는걸 사용하게 되는데 해당 라이브러리에서 대신 해주므로 리액트 프로젝트에서 subscribe 를 직접 해야 되는 일은 특별한 상황을 제외하고는 거의 없습니다 (예: 프로젝트에서 리액트 외의 라이브러리에 리덕스 연동 등..)

우리는, 상태 업데이트가 발생 할 때마다 우리가 준비한 render 함수를 호출시켜야겠죠? 한번 해봅시다.

`index.js`

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("light")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};

// 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, // 기존의 값은 그대로 두면서
        light: !state.light // light 값 반전시키기
      };
    case INCREMENT:
      return {
        ...state,
        counter: state.counter + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      // 지원하지 않는 액션의 경우 상태 유지
      return state;
  }
}

// 스토어 만들기
const store = createStore(reducer);

// render 함수 만들기
const render = () => {
  const state = store.getState(); // 현재 상태를 가져옵니다.
  const { light, counter } = state; // 편의상 비구조화 할당
  if (light) {
    lightDiv.style.background = "green";
    switchButton.innerText = "끄기";
  } else {
    lightDiv.style.background = "gray";
    switchButton.innerText = "켜기";
  }
  counterHeadings.innerText = counter;
};

render();

// **** 구독하기
store.subscribe(render);
```

<br/>

### 이벤트 달아주기, 액션 발생시키기

액션을 발생시키는것을 우리는 디스패치 (dispatch) 라고 부릅니다. 디스패치를 할 땐, 스토어의 내장함수 dispatch 를 사용합니다. 파라미터는 액션 객체를 전달하죠.

다음과 같이, 각 버튼에 이벤트를 달아주세요. 이벤트 함수에서는 dispatch 함수를 사용하여 액션을 스토어한테 전달해주겠습니다.

```javascript
import { createStore } from "redux";

const lightDiv = document.getElementsByClassName("light")[0];
const switchButton = document.getElementById("switch-btn");

const counterHeadings = document.getElementsByTagName("h1")[0];
const plusButton = document.getElementById("plus-btn");
const minusButton = document.getElementById("minus-btn");

// 액션 타입 정의
const TOGGLE_SWITCH = "TOGGLE_SWITCH";
const INCREMENT = "INCREMENT";
const DECREMENT = "DECREMENT";

// 액션 생성함수 정의
const toggleSwitch = () => ({ type: TOGGLE_SWITCH });
const increment = diff => ({ type: INCREMENT, diff });
const decrement = () => ({ type: DECREMENT });

// 초깃값 설정
const initialState = {
  light: false,
  counter: 0
};

// 리듀서 함수 정의
function reducer(state = initialState, action) {
  switch (action.type) {
    case TOGGLE_SWITCH:
      return {
        ...state, // 기존의 값은 그대로 두면서
        light: !state.light // light 값 반전시키기
      };
    case INCREMENT:
      return {
        ...state,
        counter: state.counter + action.diff
      };
    case DECREMENT:
      return {
        ...state,
        counter: state.counter - 1
      };
    default:
      // 지원하지 않는 액션의 경우 상태 유지
      return state;
  }
}

// 스토어 만들기
const store = createStore(reducer);

// render 함수 만들기
const render = () => {
  const state = store.getState(); // 현재 상태를 가져옵니다.
  const { light, counter } = state; // 편의상 비구조화 할당
  if (light) {
    lightDiv.style.background = "green";
    switchButton.innerText = "끄기";
  } else {
    lightDiv.style.background = "gray";
    switchButton.innerText = "켜기";
  }
  counterHeadings.innerText = counter;
};

render();

//  구독
store.subscribe(render);

// **** 이벤트 달아주기, 액션 발생 시키기
switchButton.onclick = () => {
  store.dispatch(toggleSwitch());
};

plusButton.onclick = () => {
  store.dispatch(increment(5));
};

minusButton.onclick = () => {
  store.dispatch(decrement());
};
```

<p><a target="_blank" href="https://codesandbox.io/s/vvzqnvw17y"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit Vanilla JS Redux Boilerplate"></a></p>

한번 버튼들을 눌러보세요. 값이 잘 바뀌나요? 모두 다 정상 작동한다면, 개념 미리 정리하기 에서 나왔던 키워드들을 다시 한번 쭉 훑어보시고, 다음 포스트를 읽어주세요.

<br/><br/>

> 참조 - 퍼온글 
> - [Redux (2) 리액트 없이 쓰는 리덕스](https://velog.io/@velopert/Redux-2-%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%97%86%EC%9D%B4-%EC%93%B0%EB%8A%94-%EB%A6%AC%EB%8D%95%EC%8A%A4-cijltabbd7)
