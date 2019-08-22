# Vuex 시작하기 2 - Getters 와 Mutations

<br/>

출처 :: [Vuex 시작하기 2 - Getters 와 Mutations](https://joshua1988.github.io/web-development/vuejs/vuex-getters-mutations/)

<br/>

## Getters 란?

중앙 데이터 관리식 구조에서 발생하는 문제점 중 하나는 각 컴포넌트에서 Vuex 의 데이터를 접근할 때 중복된 코드를 반복호출 하게 되는 것이다. 
예를 들어, 아래와 같은 코드가 있다.

```javascript
// App.vue
computed: {
  doubleCounter() {
    return this.$store.state.counter * 2;
  }
},

// Child.vue
computed: {
  doubleCounter() {
    return this.$store.state.counter * 2;
  }
},
```

여러 컴포넌트에서 같은 로직을 비효율적으로 중복 사용하고 있다. 
이 때, Vuex 의 데이터 (state) 변경을 각 컴포넌트에서 수행하는 게 아니라, Vuex 에서 수행하도록 하고 각 컴포넌트에서 수행 로직을 호출하면, 
코드 가독성도 올라가고 성능에서도 이점이 생긴다.

```javascript
// store.js (Vuex)
getters: {
  doubleCounter: function (state) {
    return state.counter * 2;
  }
},

// App.vue
computed: {
  doubleCounter() {
    return this.$store.getters.doubleCounter;
  }
},

// Child.vue
computed: {
  doubleCounter() {
    return this.$store.getters.doubleCounter;
  }
},
```

![vuex-getters](https://joshua1988.github.io/images/posts/web/vuejs/vuex-2/vuex-getters.png)

Getters 를 적용해도 비슷해 보이는가? 이건 정말 간단한 예제일 뿐이다. 만약

```javascript
this.store.state.todos.filter(todo => todo.done)...
```

등의 복잡한 로직이라면 왜 Getters 를 쓰는게 편할지 납득이 갈 것이다.

<br/>

## Getters 등록을 위한 코드 정리

지난 튜토리얼 에 이어서 getters 를 추가해보자.

먼저, 지난번 코드에서 정리해야 하는 부분은 아래와 같다.

```javascript
<!-- App.vue -->
<div id="app">
  Parent counter : {{ this.$store.state.counter }}
  <!-- ... -->
</div>
```

Vue 공식 사이트에서 [언급](https://kr.vuejs.org/v2/guide/computed.html#computed-%EC%86%8D%EC%84%B1)된 것처럼 
Template 의 표현식은 최대한 간소화해야 한다.

따라서,

```html
<!-- App.vue -->
<div id="app">
  Parent counter : {{ parentCounter }}
  <!-- ... -->
</div>

<!-- Child.vue -->
<div>
  Child counter : {{ childCounter }}
  <!-- ... -->
</div>
```

```javascript
// App.vue
computed: {
  parentCounter() {
    return this.$store.state.counter;
  }
},

// Child.vue
computed: {
  childCounter() {
    return this.$store.state.counter;
  }
},
```

computed 속성을 활용함으로써 Template 코드가 더 간결해지고, 가독성이 좋아졌다.

<br/>

## Getters 등록

여기서 한술 더 떠서 getters 를 Vuex 에 추가한다.

```javascript
// store.js
export const store = new Vuex.Store({
  // ...
  getters: {
    getCounter: function (state) {
      return state.counter;
    }
  }
});
```

<br/>

## Getters 사용

등록된 getters 를 각 컴포넌트에서 사용하려면 this.$store 를 이용하여 getters 에 접근한다.

```javascript
// App.vue
computed: {
  parentCounter() {
    this.$store.getters.getCounter;
  }
},

// Child.vue
computed: {
  childCounter() {
    this.$store.getters.getCounter;
  }
},
```

이렇게 getters 를 Vuex 에 등록하고 사용하였다. 
참고로, `computed` 의 장점인 Caching 효과는 단순히 state 값을 반환하는 것이 아니라, 
getters 에 선언된 속성에서 filter(), reverse() 등의 추가적인 계산 로직이 들어갈 때 발휘된다.

