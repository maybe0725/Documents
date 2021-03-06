# Redux (1) 소개 및 개념정리

<br/>

### 1-1. 리덕스 소개

리덕스는, 가장 사용률이 높은 상태관리 라이브러리입니다. 리덕스를 사용하면, 여러분이 만들게 될 컴포넌트들의 상태 관련 로직들을 다른 파일들로 분리시켜서 더욱 효율적으로 관리 할 수 있습니다. 또한, 컴포넌트끼리 상태를 공유하게 될 때 여러 컴포넌트를 거치지 않고도 손쉽게 상태 값을 전달 할 수 있습니다.

추가적으로, 리덕스의 미들웨어라는 기능을 통하여 비동기 작업, 로깅 등의 확장적인 작업들을 더욱 쉽게 할 수도 있게 해줍니다. 이 미들웨어에 대해서는 나중에 다뤄보게 됩니다!

<br/>

### 필요성 파악하기

리덕스는 글로벌 상태 관리를 하게 될 때 굉장히 효과적입니다. 물론, 리덕스를 사용하는것이 유일한 솔루션은 아닙니다. Context API 를 통해서도 동일한 작업을 할 수 있다는것을 미리 알려드립니다.

먼저, 투두 리스트 앱의 컴포넌트 구조를 살펴보겠습니다.

<br/>

투두리스트에서는, CreateForm 에서 우리가 값을 투두 아이템을 추가하면, App 에서 CreateForm 에게 전달해준 handleCreate 함수가 호출되고, 이 함수가 호출 되면 App 의 state 안에 들어있는 todos 값이 업데이트 됩니다.

todos 값이 업데이트 되면, 해당 값이 TodoList 한테 전달되어서 우리가 만든 투두아이템들이 모두 잘 나타나게 되겠죠.

위 구조를 보시면, CreateForm 와, TodoList 간의 데이터 교류를 하기 위해서 App 이라는 부모 컴포넌트가 중간자 역할을 해주었습니다.

위와 같이, 간단한 구조를 갖추고 있는 프로젝트는 글로벌 상태 관리를 위하여 따로 상태 관리 라이브러리를 사용하실 필요가 없습니다.

하지만, 컴포넌트 구조가 조금만 더 복잡해지면 어떨까요?

<br/>

Root 컴포넌트에는 something 이라는 상태 값이 있고, onDoSomething 이라는 함수가 something 값에 변화를 줍니다.

onDoSomething 은 Root -> B -> H 로 전달되고, H 에서 이벤트가 발생하여 이 함수가 호출되면 something 이 Root -> A -> E -> F 로 전달됩니다.

props 가 필요한 곳으로 제대로 전달되게 하기 위하여, 실제로는 해당 props 를 사용하지 않는 컴포넌트를 거쳐가야 한다는 것은 리렌더링 하게 될 때 비효율적이기도하고, 굉장히 귀찮은 작업이기도 합니다. 상위 컴포넌트에서 props 이름을 바꿔준다면 그 아래에도 쭉 바꿔줘야 하니까요.

리덕스가 있다면, 다음과 같은 구조로 작업을 진행 할 수 있게 됩니다.

<br/>

앱이 지니고 있는 상태와, 상태 변화 로직이 들어있는 스토어를 통하여, 우리가 원하는 컴포넌트에 원하는 상태값과 함수를 직접 주입해줄 수 있게 됩니다.

이런 식으로, 더 쉬운 글로벌 상태 관리를 위하여 리덕스를 사용하기도 하고, 조금 더 체계적이고 편리한 상태 관리를 하기 위하여 사용을 하는데, 후자의 경우엔 실제로 사용을 해봐야 경험을 해볼 수 있을 것입니다. 그럼, 계속해서 진행해볼까요?

<br/>

### 1-2. 개념 미리 정리하기

앞으로 접하게 될 키워드들에 대해서 미리 알아보는 시간을 가져보겠습니다. 대략적인 개념만 간략히 알아보는 것 이므로, 도중에 잘 이해가 안가는게 있더라도, 나중에 직접 사용해본 다음에 이 섹션으로 다시 돌아와서 다시 읽으시면 이해가 더욱 잘 될 것입니다.

<br/>

### 액션 (Action)

상태에 어떠한 변화가 필요하게 될 땐, 우리는 액션이란 것을 발생시킵니다. 이는, 하나의 객체로 표현되는데요, 액션 객체는 다음과 같은 형식으로 이뤄져있습니다.

```javascript
{
  type: "TOGGLE_VALUE";
}
```

액션 객체는 `type` 필드를 필수적으로 가지고 있어야하고 그 외의 값들은 개발자 마음대로 넣어줄 수 있습니다.

예시:

```javascript
{
  type: "ADD_TODO",
  data: {
    id: 0,
    text: "리덕스 배우기"
  }
}
```

```javascript
{
  type: "CHANGE_INPUT",
  text: "안녕"
}
```

<br/>

### 액션 생성함수 (Action Creator)

액션 생성함수는, 액션을 만드는 함수입니다. 단순히 파라미터를 받아와서 액션 객체 형태로 만들어주죠.

```javascript
function addTodo(data) {
  return {
    type: "ADD_TODO",
    data
  };
}

// 화살표 함수로도 만들 수 있습니다.
const changeInput = text => ({
  type: "CHANGE_INPUT",
  text
});
```

<br/>

### 리듀서 (Reducer)

리듀서는 변화를 일으키는 함수입니다. 리듀서는 두가지의 파라미터를 받아옵니다.

```javascript
function reducer(state, action) {
  // 상태 업데이트 로직
  return alteredState;
}
```

리듀서는, 현재의 상태와, 전달 받은 액션을 참고하여 새로운 상태를 만들어서 반환합니다. 자세한건, 추후 직접 구현하면서 알아보겠습니다.

<br/>

### 스토어 (Store)

리덕스에서는 한 애플리케이션 당 하나의 스토어를 만들게 됩니다. 스토어 안에는, 현재의 앱 상태와, 리듀서가 들어가있고, 추가적으로 몇가지 내장 함수들이 있습니다.

<br/>

### 디스패치 (dispatch)

디스패치는 스토어의 내장함수 중 하나입니다. 디스패치는, 액션을 발생 시키는 것 이라고 이해하시면 됩니다. dispatch 라는 함수에는 액션을 파라미터로 전달합니다.. dispatch(action) 이런식으로 말이죠.

그렇게 호출을 하면, 스토어는 리듀서 함수를 실행시켜서 해당 액션을 처리하는 로직이 있다면 액션을 참고하여 새로운 상태를 만들어줍니다.

### 구독 (subscribe)

구독 또한 스토어의 내장함수 중 하나입니다. subscribe 함수는, 함수 형태의 값을 파라미터로 받아옵니다. subscribe 함수에 특정 함수를 전달해주면, 액션이 디스패치 되었을 때 마다 전달해준 함수가 호출됩니다.

<br/>
<br/>

> 참조 - 퍼온글 
> - [Redux (1) 소개 및 개념정리](https://velog.io/@velopert/Redux-1-%EC%86%8C%EA%B0%9C-%EB%B0%8F-%EA%B0%9C%EB%85%90%EC%A0%95%EB%A6%AC-zxjlta8ywt)
