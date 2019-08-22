# Vuex 시작하기 3 - Actions 와 폴더 구조화

<br/>

참조 :: [Vuex 시작하기 3 - Actions 와 폴더 구조화](https://joshua1988.github.io/web-development/vuejs/vuex-actions-modules/)

<br/>

## Actions 란?

Mutations 에는 순차적인 로직들만 선언하고 Actions 에는 비 순차적 또는 비동기 처리 로직들을 선언한다. 
그렇다면 왜 처리 로직의 성격에 따라 Mutations 과 Actions 로 나눠 등록해야 할까?

Mutations 에 대해 잠깐 짚어보면, Mutations 의 역할 자체가 State 관리에 주안점을 두고 있다. 
상태관리 자체가 한 데이터에 대해 여러 개의 컴포넌트가 관여하는 것을 효율적으로 관리하기 위함인데 
Mutations 에 비동기 처리 로직들이 포함되면 같은 값에 대해 여러 개의 컴포넌트에서 변경을 요청했을 때, 그 변경 순서 파악이 어렵기 때문이다.

이러한 문제를 방지하기 위해 **비동기 처리 로직은 Actions** 에 **동기 처리 로직은 Mutations** 에 나눠 구현한다.

따라서, `setTimeout()` 이나 서버와의 http 통신 처리 같이 결과를 받아올 타이밍이 예측되지 않은 로직은 Actions 에 선언한다.

<br/>

## Actions 등록

Vuex 에 Actions 를 등록하는 방법은 다른 속성과 유사하다. actions 를 선언하고 action method 를 추가해준다.

```js
// store.js
export const store = new Vuex.Store({
  // ...
  mutations: {
    addCounter: function (state, payload) {
      return state.counter++;
    }
  },
  actions: {
    addCounter: function (context) {
      // commit 의 대상인 addCounter 는 mutations 의 메서드를 의미한다.
      return context.commit('addCounter');
    }
  }
});
```

상태가 변화하는 걸 추적하기 위해 actions 는 결국 mutations 의 메서드를 호출(commit) 하는 구조가 된다.

```js
// store.js
export const store = new Vuex.Store({
  actions: {
    getServerData: function (context) {
      return axios.get("sample.json").then(function() {
        // ...
      });
    },
    delayFewMinutes: function (context) {
      return setTimeout(function () {
        commit('addCounter');
      }, 1000);
    }
  }
});
```

위처럼 HTTP get 요청이나 setTimeout 과 같은 비동기 처리 로직들은 actions 에 선언해준다.

<br/>

## Actions 사용

앞에서는 mutations 를 이용하여 counter 를 하나씩 늘렸다. 
이번엔 actions 를 이용해보자. actions 를 호출할 때는 아래와 같이 dispatch() 를 이용한다.

```js
// App.vue
methods: {
  // Mutations 를 이용할 때
  addCounter() {
    this.$store.commit('addCounter');
  }
  // Actions 를 이용할 때
  addCounter() {
    this.$store.dispatch('addCounter');
  }
},
```

전체 구조도에서 dispatch 의 동작을 보면

![vuex-actions](https://joshua1988.github.io/images/posts/web/vuejs/vuex-3/vuex-actions.png)

<br/>

## Actions 에 인자 값 넘기기

Actions 에 인자를 넘기는 방법은 Mutations 와 유사하다.

```html
<!-- by 와 duration 등의 여러 인자 값을 넘길 경우, 객체안에 key - value 형태로 여러 값을 넘길 수 있다 -->
<button @click="asyncIncrement({ by: 50, duration: 500 })">Increment</button>
```

```js
export const store = new Vuex.Store({
  actions: {
    // payload 는 일반적으로 사용하는 인자 명
    asyncIncrement: function (context, payload) {
      return setTimeout(function () {
        context.commit('increment', payload.by);
      }, payload.duration);
    }
  }
})
```

<br/>

## mapActions

mapGetters, mapMutations 헬퍼 함수들과 마찬가지로 mapActions 도 동일한 방식으로 사용할 수 있다.

```js
import {mapActions} from 'vuex';

export default {
  methods: {
    ...mapActions([
      'asyncIncrement',
      'asyncDecrement'
    ])
  },
}
```

<br/>

## 폴더 구조화 & Namespacing

중간 크기 이상의 복잡한 앱을 제작할 때 `getters & mutations & actions` 의 이름을 유일하게 정하지 않으면 namespace 충돌이 난다. 
**따라서, 네임스페이스를 구분하기 위해 `types.js` 로 각 속성의 이름들을 빼고 `store.js` 와 각 컴포넌트에 import 하여 사용하는 방법이 있다.**
혹은 modules 라는 폴더로 만들어 각 단위별로 파일을 쪼개서 관리하는 방법도 있다.

![vuex-folder-structure](https://joshua1988.github.io/images/posts/web/vuejs/vuex-3/vuex-folder-structure.png)

생각보다 복잡하므로 앱이 커서 중형 이상의 앱에서만 사용하는게 좋을 듯하다. 간단한 화면 개발에는 오히려 배보다 배꼽이 클 수 있다.

<br/>

## 마무리

지난 2개의 글과 함께 총 3편의 Vuex 관련 글을 통해, Vue 로 앱을 개발할 때 더 효율적으로 코드와 데이터를 관리할 수 있는 
**상태관리**에 대해 알아보았습니다. 
중형 이상의 앱에는 필수적으로 써야하는 보조 라이브러리라고 보실 수 있는데요. 
Vuex 가 가져다 주는 이점도 크지만, 정말 간단한 화면을 만들 때는 오히려 초기 세팅하는데 시간이 많이 걸릴 수 있으니 유의 하시기 바랍니다 :)
