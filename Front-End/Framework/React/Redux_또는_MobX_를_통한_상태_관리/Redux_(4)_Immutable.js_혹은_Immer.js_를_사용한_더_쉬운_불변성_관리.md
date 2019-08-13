# Redux (4) Immutable.js 혹은 Immer.js 를 사용한 더 쉬운 불변성 관리

<br/>

### 4-1. Immutable.js 사용하기

Immutable.js 는 불변성을 유지해줘야 하는 객체의 값을 더 쉽게 업데이트 할 수 있게 해줍니다.

Immutable.js 의 사용법은 [여기](https://react-immutable.vlpt.us/01.html)서 더 자세히 보실 수 있습니다.

우선, 설치를 해주겠습니다.

```
[yarn]
$ yarn add immutable

[npm]
$ npm install immutable
```

<br/>

### counter.js 모듈에 Immutable 적용

우선 initialState 를 Immutable 의 Map 형태로 변환해주고, 리듀서쪽에서는 ... spread 연산자를 사용하는것이 아니라, Immutable 의 내장함수들을 사용하여 업데이트를 해주겠습니다.

`src/store/modulies/counter.js`

```javascript
import { Map } from "immutable";

// 액션 타입 정의
const CHANGE_COLOR = "counter/CHANGE_COLOR";
const INCREMENT = "counter/INCREMENT";
const DECREMENT = "counter/DECREMENT";

// 액션 생섬함수 정의
export const changeColor = color => ({ type: CHANGE_COLOR, color });
export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });

// **** Immutable 의 Map 으로 감싸기
const initialState = Map({
  color: "red",
  number: 0
});

// 리듀서 작성
export default function counter(state = initialState, action) {
  switch (action.type) {
    case CHANGE_COLOR:
      // **** set 으로 특정 필드의 값을 설정
      return state.set("color", action.color);
    case INCREMENT:
      // **** update 는 현재 값을 읽어온 다음에 함수에서 정의한 업데이트 로직에 따라 값 변경
      return state.update("number", number => number + 1);
    case DECREMENT:
      // **** 마찬가지
      return state.update("number", number => number - 1);
    default:
      return state;
  }
}
```

Immutable 을 사용하면 업데이트를 하게 될 때 위와 같이 내장 함수들을 활용하여 간단하게 할 수 있는 대신에, 값이 일반 객체가 아니기 때문에 상태에서 값을 조회하고 싶을 때 `counter.color` 이런식으로는 값을 조회하지 못하고 `counter.get('color')` 이렇게 해줘야 한다는 번거로움이 있습니다.

그래서, 컨테이너 컴포넌트들도 조금 수정을 해주어야 합니다.

<br/>

### CounterContainer 와 PaletteContainer 수정

`src/containers/CounterContainer.js`

```javascript
import React, { Component } from "react";
import { connect } from "react-redux";
import Counter from "../components/Counter";
import { increment, decrement } from "../store/modules/counter";

class CounterContainer extends Component {
  handleIncrement = () => {
    this.props.increment();
  };
  handleDecrement = () => {
    this.props.decrement();
  };
  render() {
    const { color, number } = this.props;
    return (
      <Counter
        color={color}
        value={number}
        onIncrement={this.handleIncrement}
        onDecrement={this.handleDecrement}
      />
    );
  }
}

const mapStateToProps = ({ counter }) => ({
  // **** .get 을 사용해서 값 조회
  color: counter.get("color"),
  number: counter.get("number")
});

// 함수가 아닌 객체 설정시 자동 bindActionCreators 됨
const mapDispatchToProps = { increment, decrement };

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(CounterContainer);
```

`src/containers/PaletteContainer.js`

```javascript
import React, { Component } from "react";
import { connect } from "react-redux";
import Palette from "../components/Palette";
import { changeColor } from "../store/modules/counter";

class PaletteContainer extends Component {
  handleSelect = color => {
    const { changeColor } = this.props;
    console.log("what");
    changeColor(color);
  };

  render() {
    const { color } = this.props;
    return <Palette onSelect={this.handleSelect} selected={color} />;
  }
}

// props 로 넣어줄 스토어 상태값
const mapStateToProps = state => ({
  color: state.counter.get("color") // **** .get 으로 조회
});

// props 로 넣어줄 액션 생성함수
const mapDispatchToProps = dispatch => ({
  changeColor: color => dispatch(changeColor(color))
});

// 컴포넌트에 리덕스 스토어를 연동해줄 때에는 connect 함수 사용
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(PaletteContainer);
```

<br/>

### waiting.js 모듈에 Immutable.js 적용

이 모듈에서는 배열이 있으므로, 배열은 Immutable 의 List 형태로 값을 넣어줍니다.

`src/modules/waiting.js`

```javascript
import { createAction, handleActions } from "redux-actions";
import { List, Map } from "immutable"; // **** Immutable 의  List 와 Map 불러오기

const CHANGE_INPUT = "waiting/CHANGE_INPUT"; // 인풋 값 변경
const CREATE = "waiting/CREATE"; // 명단에 이름 추가
const ENTER = "waiting/ENTER"; // 입장
const LEAVE = "waiting/LEAVE"; // 나감

let id = 3;
// createAction 으로 액션 생성함수 정의
export const changeInput = createAction(CHANGE_INPUT, text => text);
export const create = createAction(CREATE, text => ({ text, id: id++ }));
export const enter = createAction(ENTER, id => id);
export const leave = createAction(LEAVE, id => id);

// **** Immutable 형태로 변환
const initialState = Map({
  input: "",
  list: List([
    Map({
      id: 0,
      name: "홍길동",
      entered: true
    }),
    Map({
      id: 1,
      name: "콩쥐",
      entered: false
    }),
    Map({
      id: 2,
      name: "팥쥐",
      entered: false
    })
  ])
});

// handleActions 로 리듀서 함수 작성
// **** 내부 업데이트 로직 모두 Immutable 내장함수로 변경
export default handleActions(
  {
    [CHANGE_INPUT]: (state, action) => state.set("input", action.payload),
    [CREATE]: (state, action) =>
      // list 값을 조회한다음에
      state.update("list", list =>
        // list 에 새로운 Map 을 추가
        list.push(
          Map({
            id: action.payload.id,
            name: action.payload.text,
            entered: false
          })
        )
      ),
    [ENTER]: (state, action) => {
      // 인덱스를 찾고
      const index = state
        .get("list")
        .findIndex(item => item.get("id") === action.payload);
      // 특정 인덱스의 entered 필드 값을 반전
      return state.updateIn(["list", index, "entered"], entered => !entered);
    },
    [LEAVE]: (state, action) => {
      // 인덱스를 찾고
      const index = state
        .get("list")
        .findIndex(item => item.get("id") === action.payload);
      return state.deleteIn(["list", index]); // 특정 인덱스 제거
    }
  },
  initialState
);
```

업데이트 방식이, Immutable 의 내장함수를 사용하는것으로 많이 바뀌었는데, Immutable.js 의 내장함수가 익숙해지기 전에는 조금 낯설을 수도 있습니다.

<br/>

### WaitingListContainer 와 WaitingList 컴포넌트 Immutable 호환

WaitingListContainer 와 WaitingList 에서 Immutable 데이터들을 제대로 처리해줄 수 있게끔 해주겠습니다.

`src/containers/WaitingListContainer.js`

```javascript
import React, { Component } from "react";
import { connect } from "react-redux";
import { bindActionCreators } from "redux";
import * as waitingActions from "../store/modules/waiting";
import WaitingList from "../components/WaitingList";

class WaitingListContainer extends Component {
  // 인풋 변경 이벤트
  handleChange = e => {
    const { WaitingActions } = this.props;
    WaitingActions.changeInput(e.target.value);
  };
  // 등록 이벤트
  handleSubmit = e => {
    e.preventDefault();
    const { WaitingActions, input } = this.props;
    WaitingActions.create(input); // 등록
    WaitingActions.changeInput(""); // 인풋 값 초기화
  };
  // 입장
  handleEnter = id => {
    const { WaitingActions } = this.props;
    WaitingActions.enter(id);
  };
  // 나가기
  handleLeave = id => {
    const { WaitingActions } = this.props;
    WaitingActions.leave(id);
  };
  render() {
    const { input, list } = this.props;
    return (
      <WaitingList
        input={input}
        waitingList={list}
        onChange={this.handleChange}
        onSubmit={this.handleSubmit}
        onEnter={this.handleEnter}
        onLeave={this.handleLeave}
      />
    );
  }
}

const mapStateToProps = ({ waiting }) => ({
  // **** .get 사용
  input: waiting.get("input"),
  list: waiting.get("list")
});

// 이런 구조로 하면 나중에 다양한 리덕스 모듈을 적용해야 하는 상황에서 유용합니다.
const mapDispatchToProps = dispatch => ({
  WaitingActions: bindActionCreators(waitingActions, dispatch)
  // AnotherActions: bindActionCreators(anotherActions, dispatch)
});
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(WaitingListContainer);
```

WaitingList 에서도 마찬가지로 비슷한작업을 해주셔야 합니다.

`src/components/WaitingList.js`

```javascript
import React from "react";
import "./WaitingList.css";

const WaitingItem = ({ text, entered, onEnter, onLeave }) => {
  return (
    <li>
      <div className={`text ${entered ? "entered" : ""}`}>{text}</div>
      <div className="buttons">
        <button onClick={onEnter}>입장</button>
        <button onClick={onLeave}>나감</button>
      </div>
    </li>
  );
};

const WaitingList = ({
  input,
  waitingList,
  onChange,
  onSubmit,
  onEnter,
  onLeave
}) => {
  const waitingItems = waitingList.map(w => (
    <WaitingItem
      // **** .get 사용
      key={w.get("id")}
      text={w.get("name")}
      entered={w.get("entered")}
      id={w.get("id")}
      onEnter={() => onEnter(w.get("id"))}
      onLeave={() => onLeave(w.get("id"))}
    />
  ));
  return (
    <div className="WaitingList">
      <h2>대기자 명단</h2>
      <form onSubmit={onSubmit}>
        <input value={input} onChange={onChange} />
        <button>등록</button>
      </form>
      <ul>{waitingItems}</ul>
    </div>
  );
};

export default WaitingList;
```

이제, 모든게 제대로 작동하는지 확인해주세요.

Immutable.js 는, 상태 객체의 구조가 다음과 같이 매우 복잡해지는 경우:

```javascript
{
  something: {
    inside: {
      here: {
        hello: '여기를 바꾸고싶을때'
      }
    },
    foo: 'bar',
    foobar: 'asdf'
  }
}
```

`state.setIn(['something', 'inside', 'here', 'hello'], '새로운 값')` 이런식으로 간단하게 처리를 해줄 수 있다는 장점이 있긴 하지만, 이 값을 조회하게 될 때 언제나 .get 을 해야 된다는점은 꽤나 번거롭습니다.

그럼에도 불구하고, 업데이트의 편리성 때문에, 그리고 페이스북에서 만든 라이브러리이기도 해서 사용률은 굉장히 높습니다.

하지만 Immutable.js 의 사용은 필수는 아니고, 만약에 상태의 구조를 최대한 깊지 않게 진행하고 우리가 이전헤 했던 것 처럼 배열의 경우엔 map, filter 내장함수를 잘 응용하면 충분히 깔끔하게 코드를 작성 할 수 있습니다.

추가적으로, Immutable 외에도 다른 불변성 유지 관리 라이브러리들이 있는데, 그 중에서 Immer.js 라는 라이브러리는 정말로 편리합니다. 한번 다음 섹션에서 사용해보겠습니다!

<br/>

### 4-2. Immer 로 불변성 유지하기

Immer 는 굉장히 편리한 불변성 유지 라이브러리입니다.

먼저 설치를 해주세요.

```
[yarn]
$ yarn add immer

[npm]
$ npm install immer
```

<br/>

### Immutable.js 비활성화

이전 섹션에서 Immutable.js 를 적용하면서, 리듀서도 변경을 했고 컨테이너랑 컴포넌트쪽에서 .get 을 사용하는 형태로 수정을 했었는데요, 이 작업들을 다시 원상복구해주겠습니다.

직접 이전에 Immutable.js 를 사용하지 않던 상태로 되돌려 놓으셔도 되고, [여기](https://github.com/vlpt-playground/learn-redux/tree/02-waitinglist/src)에서 필요한 파일들을 복사/붙여넣기 해주세요

- components/WaitingList.js
- containers/\*.js
- store/modules/\*.js

<br/>

### Immer 사용법

Immer 사용법은, 너무 쉽습니다. 마치 불변성에 대해서 신경쓰지 않는 것 처럼 데이터를 업데이트 해주면, 라이브러리가 알아서 불변성 유지를 해주면서 업데이트를 해줍니다.

```javascript
import produce from "immer";

const baseState = [
  {
    todo: "Learn typescript",
    done: true
  },
  {
    todo: "Try immer",
    done: false
  }
];

const nextState = produce(baseState, draftState => {
  draftState.push({ todo: "Tweet about it" });
  draftState[1].done = true;
});
```

<br/>

### counter.js 모듈에 Immer 적용

`src/store/modules/counter.js`

```javascript
import produce from "immer"; // **** immer 불러오기

// 액션 타입 정의
const CHANGE_COLOR = "counter/CHANGE_COLOR";
const INCREMENT = "counter/INCREMENT";
const DECREMENT = "counter/DECREMENT";

// 액션 생섬함수 정의
export const changeColor = color => ({ type: CHANGE_COLOR, color });
export const increment = () => ({ type: INCREMENT });
export const decrement = () => ({ type: DECREMENT });

// 초기상태 정의
const initialState = {
  color: "red",
  number: 0
};

// 리듀서 작성
// **** 내부 업데이트 로직 모두 수정
export default function counter(state = initialState, action) {
  switch (action.type) {
    case CHANGE_COLOR:
      return produce(state, draft => {
        draft.color = action.color;
      });
    case INCREMENT:
      return produce(state, draft => {
        draft.number++;
      });
    case DECREMENT:
      return produce(state, draft => {
        draft.number--;
      });
    default:
      return state;
  }
}
```

어떤가요? 너무 간단하죠? 신기하게도 잘 됩니다..

<br/>

### waiting.js 모듈에 Immer 적용

이렇게 배열이 있는 곳에서 사용하면, 편리함을 실감하실 수 있습니다.

`src/modules/waiting.js`

```javascript
import { createAction, handleActions } from "redux-actions";
import produce from "immer"; // **** Immer 불러오기

const CHANGE_INPUT = "waiting/CHANGE_INPUT"; // 인풋 값 변경
const CREATE = "waiting/CREATE"; // 명단에 이름 추가
const ENTER = "waiting/ENTER"; // 입장
const LEAVE = "waiting/LEAVE"; // 나감

let id = 3;
// createAction 으로 액션 생성함수 정의
export const changeInput = createAction(CHANGE_INPUT, text => text);
export const create = createAction(CREATE, text => ({ text, id: id++ }));
export const enter = createAction(ENTER, id => id);
export const leave = createAction(LEAVE, id => id);

// **** 초기 상태 정의
const initialState = {
  input: "",
  list: [
    {
      id: 0,
      name: "홍길동",
      entered: true
    },
    {
      id: 1,
      name: "콩쥐",
      entered: false
    },
    {
      id: 2,
      name: "팥쥐",
      entered: false
    }
  ]
};

// handleActions 로 리듀서 함수 작성
// **** 내부 업데이트 로직 모두 업데이트
export default handleActions(
  {
    [CHANGE_INPUT]: (state, action) =>
      produce(state, draft => {
        draft.input = action.payload;
      }),
    [CREATE]: (state, action) =>
      produce(state, draft => {
        draft.list.push({
          id: action.payload.id,
          name: action.payload.text,
          entered: false
        });
      }),
    [ENTER]: (state, action) =>
      produce(state, draft => {
        const item = draft.list.find(item => item.id === action.payload);
        item.entered = !item.entered;
      }),
    [LEAVE]: (state, action) =>
      produce(state, draft => {
        draft.list.splice(
          draft.list.findIndex(item => item.id === action.payload),
          1
        );
      })
  },
  initialState
);
```

정말 간단하죠? 이러한 편리성 때문에, 정말 강력 추천드리고 싶고, 저의 경우엔 새로 작성하는 리덕스 모듈에서는 Immer 라이브러리를 애용하고있습니다.

<p><a target="_blank" href="https://codesandbox.io/s/96m70mj6p"><img src="https://codesandbox.io/static/img/play-codesandbox.svg" alt="Edit colorful-counter"></a></p>

<br/>
<br/>

> 참조 - 퍼온글
>
> - [Redux (4) Immutable.js 혹은 Immer.js 를 사용한 더 쉬운 불변성 관리](https://velog.io/@velopert/20180908-1909-%EC%9E%91%EC%84%B1%EB%90%A8-etjltaigd1)
