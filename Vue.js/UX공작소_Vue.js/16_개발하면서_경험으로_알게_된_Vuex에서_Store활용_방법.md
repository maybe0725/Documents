# 개발하면서 경험으로 알게 된 Vuex에서 Store활용 방법

<br/>

원본글 출처

- [개발하면서 경험으로 알게 된 Vuex에서 Store활용 방법](https://ux.stories.pe.kr/149)

<br/>

Vue의 개발을 편리하게 도와 주는 공식 툴 중에 Vuex가 있습니다.
`Vuex의 주요 기능은 개발하는 애플리케이션의 모든 컴포넌트에 대한 중앙 집중식 저장소 역활 및 관리` 입니다.

만약 이게 없다면 컴포넌트간 데이터를 주고받기 위해서 `부모는 자식에서 props의 방법`으로, `자식은 부모에게 Emit event의 방법`으로 데이터를 주고 받아야 합니다. 또한 형제 컴포넌트간 데이터를 주고 받으려면 너무 복잡해져서 `EventBus를 활용`해야 그나마 사용을 할 수 있게 됩니다. 간단한 프로젝트인 경우에야 사용이 가능하겠지만 대형 프로젝트인 경우에는 이러한 방법으로는 도저히 감당이 되지 않고 개발 후 운영을 한다고 해도 거의 맨붕이 올 것입니다.

이러한 문제를 해결해 주는 것이 `Vuex`라고 보시면 됩니다. `데이터를 store`라는 파일을 통해 관리하고 프로젝트 전체에 걸쳐 활용할 수 있게 해 주는 방법입니다.

기본적인 설명은 [해당 사이트](https://vuex.vuejs.org/kr/)에서 확인하시면 됩니다. 한글로 되어 있어서 기본적인 것을 익히는데는 어려움이 없습니다.

이번 포스트에는 `NUXT`를 활용하여 토이 프로젝트를 진행하면서 알게된 것을 스팟형태로 정리해 보려고 합니다.

참고로 NUXT에도 Vuex를 활용하고 있습니다.

<br/>

## State, Mutations, Actions, Getters의 특징

<br/>

Vuex의 핵심구성은 State, Mutations, Actions, Getters로 구성되어 있습니다. 각각 기본적인 특징과 다른점을 먼저 알기 쉽게 정리해 보겠습니다.

<br/>

### State

- state는 쉽게 말하면 프로젝트에서 공통으로 사용할 변수를 정의 합니다.
- 프로젝트 내의 모든 곳에서 참조 및 사용이 가능합니다.
- state를 통해 각 컴포넌트에서 동일한 값을 사용할 수 있습니다.

```js
export const state = () => ({
  account: null,
  isAdmin: null,
  item: null
});
```

<br/>

### Mutations

- Mutations의 주요 목적은 state를 변경시키는 역활을 합니다.
  <br/>(Mutations을 통해서만 state를 변경해야 함)
- 비동기 처리가 아니라 동기처리를 합니다.
  <br/>위의 함수가 실행되고 종료된 후 그 다음 아래의 함수가 실행됩니다.
- commit('함수명', '전달인자')으로 실행 시킬 수 있습니다.
- mutations 내에 함수 형태로 작성합니다.

```js
export const mutations = {
  currentUser(state, account) {
    // state의 account변수에 넘겨 받은 account값을 입력함
    state.account = account;
  }
};
```

<br/>

### Actions

- Actions의 주요 목적은 Mutations를 실행시키는 역활을 합니다.
  <br/>Mutations이 실행되면 state도 변경이 되겠지요.
- 동기 처리가 아니라 비동기처리를 합니다.
  <br/>순서에 상관없이 먼저 종료된 함수의 피드백을 받아 후속 처리를 하게 됩니다.
- dispatch('함수명', '전달인자')으로 실행 시킬 수 있습니다.
  <br/>ex) dispatch('함수명', '전달인자', {root:true})
- actions 내에 함수 형태로 작성합니다.
- 비동기 처리이기 때문에 콜백함수로 주로 작성합니다.

<br/>

### ● 일반 형태로 실행

```js
dispatch("setAccount", account);
```

```js
export const actions = {
  setAccount({ commit, dispatch }, account) {
    commit("currentUser", account);
    dispatch("setIsAdmin", account.uid);
  }
};
```

<br/>

### ● Components에서 then()으로 콜백함수 실행

```js
dispatch("setAccount", account).then(() => {});
```

```js
export const actions = {
  setAccount({ commit }, account) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        commit("currentUser", account);
        resolve();
      }, 1000);
    });
  }
};
```

<br/>

### Getters

- 각 Components의 계산된 속성(computed)의 공통 사용 정의라고 보시면 됩니다.
- 여러 Components에서 동일한 computed가 사용 될 경우 Getters에 정의하여 공통으로 쉽게 사용할 수 있습니다.
- 하위 모듈의 getters를 불러오기 위해서는 특이하게 this.\$store.getters["경로명/함수명"]; 을 사용해야 합니다.

```js
export const getters = {
  isAuthenticated(state) {
    // 현재 로그인 상태인지 확인 (true/false)
    return !!state.user;
  },

  getAccount(state) {
    // 회원정보 불러오기
    return state.account;
  }
};
```

<br/>

## Vuex의 데이터 처리 상관 관계

<br/>

기본적인 내용이긴 한데, Vuex에서 사용하는 클래스는 보통 state, mutation, action, getters로 구성되어 있고 아래의 구조로 구현됩니다.

아래의 형태는 기본형은 아니고 모듈별로 구현하기 위한 구조입니다. 모듈별이라 함은 store/account.js, store/member.js, store/item.js, store/category.js 등등.. 파일을 모듈 별로 작성하여 개발하는 것을 말합니다. 이것은 나중에 컴파일 과정에서 하나로 합쳐져서 사용됩니다.

<br/>

### ● 예> store/index.js

```js
import { AUTH, DB, STORAGE, GoogleProvider } from '@/services/fireinit.js';

export const strict = false;

/*
 ** STATES
 */
export const state = () => ( );

/*
 ** MUTATIONS(동기 처리)
 */
export const mutations = { };

/*
 ** ACTIONS (비동기처리)
 */
export const actions = { };

/*
 ** GETTERS
 */
export const getters = { };
```

Vuex의 특징은 아래의 순서로 데이터를 단방향으로 관리한다는 것입니다.

> 각각 컴포넌트 (**dispatch**)--> actions (**commit**)--> mutations (**state**)--> state -->모든 컴포넌트에서 활용

이것은 메뉴얼에서 나오는 기본 내용입니다.

하지만 역방향인 mutations에서 actions을 실행시킬 수 있는지, 없는지, 모듈간의 실행은 어떻게 하는지 등은 처음 vuex를 접해본 사람에게는 많은 고민이 필요할 겁니다.

<br/>

### rootState는 actions과 getters에서만 사용가능

vuex를 모듈형으로 개발 할 떄 기본적으로 state 변수 값은 동일 모듈에 있는 state만 참조하게 됩니다.

만약 내가 최상위에 있는 state 변수나 다른 모듈의 변수 값을 활용하기 위해서는 state가 아니라 `rootState`를 활용해야 합니다.

하지만 `이 rootState는 actions과 Getters의 인자로만 사용될 수 있습니다.`

만약 `Mutations에서 사용하기를 원한다면` Actions에서 받아서 Mutations쪽으로 commit해서 활용해야 합니다.

<br/>

### Mutations과 Actions의 사용가능 인자

Mutations과 Actions 내에 있는 함수에서 인자의 사용은 사용 약간의 규칙이 있습니다.

<br/>

### Mutations내 함수 인자

Mutitions내에 있는 함수의 인자는 state와 payload입니다.

기본 인자는 state만 사용할 수 있고 commit으로 넘어온 전달 인자는 payload만 있습니다.

payload는 여러개를 묶은 객체 형태로 전달 될 수 있습니다.

또한 바로 중괄호로 묶어서 개체 형태로도 전달 받을 수 있습니다.

```js
mutationA(state, payload) { }
mutationA(state, {data1, data2}) { }
```

<br/>

### Actions내 함수 인자

Actions은 비동기 처리를 합니다. 일단 실행 시키고 회신을 기다렸다가 먼저 회신 온 것 부터 처리를 합니다.

actions에서는 { rootState, state, dispatch, commit }의 기본 인자를 받을 수 있습니다.

기본 인자는 중괄호로 묶어서 전달 받습니다. 또한 payload는 Mutations와 마찮가지로 객체형태로 받을 수 있습니다.

```js
actionsA({ rootState, state, dispatch, commit }, payload) { }
```

<br/>

### Components에서 각 store 모듈에 접근하는 방법

Components에서 store에 있는 state, Mutations, Actions, Getters에 접근하는 방법은 아래와 같습니다.

<br/>

### state에 접근하는 방법

state에 접근하기 위해서는 Commponent의 **computed** 내에 작성을 해야 합니다.

```js
// 기본 접근방법
this.$store.state.items

// mapState 활용방법
computed: {
  …mapState({
    items: state => state.items,
  }),
}
```

<br/>

### Mutations에 접근하는 방법

Mutations을 실행하기 위해서는 Commponent의 **methods** 영역 내에 작성을 해야 합니다.

```js
// 기본 접근방법
this.$store.commit('경로명/함수명')

// mapMutations 활용방법
methods: {
  // methods영역에서 호출해야 함
  …mapMutations({
    // this.add()를 this.$store.commit('item/increment')에 매핑합니다.
    add: 'item/increment'
  })
}
```

<br/>

### Actions에 접근하는 방법

Actions을 실행하기 위해서는 Commponent의 **methods** 영역 내에 작성을 해야 합니다.

```js
// 기본 접근방법
this.$store.dispatch('경로명/함수명')

// mapActions 활용방법
methods: {
  // methods영역에서 호출해야 함
  …mapActions({
    // this.add()를 this.$store.dispatch('item/increment')에 매핑합니다.
    add: 'item/increment'
  })
}
```

<br/>

### Getters에 접근하는 방법

Getters을 실행하기 위해서는 Commponent의 **computed** 영역 내에 작성을 해야 합니다.

1.

1. mapGetters 활용방법

```js
// 기본 접근방법
this.$store.getters["경로명/함수명"];,
this.$store.getters.doneTodosCount,
this.$store.getters.getTodoById(2)

// mapGetters 활용방법
computed: {
  …mapGetters({
    doneCount: 'item/doneTodosCount'
  })
}
```

<br/>

### 모듈로 구성된 vuex에서 상위의 모듈에 있는 dispatch, commit를 실행시키는 방법

모듈로 구성할 경우 하위 모듈에서 **형제 또는 부모 모듈의 state**에 접근하기 위해서는 **rootState**를 사용하면 됩니다.

그러면 형제 또는 부모 모듈의 **Mutations나 Actions**를 실행시킬 경우는 어떻게 해야 하느냐!!!

세번쨰 인자에 { root: true }를 지정해 주면 됩니다

```js
dispatch("path1/actionA", payload, { root: true });
```

이제 최상위 경로인 root에서 부터 하위로 경로를 찾아 들어갈 수있습니다.
commit도 위와 같이 처리가 가능합니다.

```js
commit("path1/actionA", payload, { root: true });
```
