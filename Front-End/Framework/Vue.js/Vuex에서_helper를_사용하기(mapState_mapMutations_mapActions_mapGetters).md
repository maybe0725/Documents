# vuex에서 helper를 사용하기(mapState, mapMutations, mapActions, mapGetters)

<br/>

참고 :: [vuex에서 helper를 사용하기(mapState, mapMutations, mapActions, mapGetters)](https://kamang-it.tistory.com/entry/Vue18vuex%EC%97%90%EC%84%9C-helper%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0mapState-mapMutations-mapActions-mapGetters)

<br/>

## vuex helper 함수 종류

- `mapState` - state를 연결해주는 함수
- `mapGetters` - getters를 연결해주는 함수
- `mapMutations` - mutations를 연결해주는 함수
- `mapActions` - actions를 연결해주는 함수

<br/>

### `Vue Component template`

```html
<template>
  ...
  <div>
    <!-- store state count variable -->
    <li>count: {{ $store.state.count }}</li>
    <!-- ...mapState(["count"]) call -->
    <li>count: {{ count }}</li>
    <!-- store action group logout method call -->
    <v-btn @click="$store.dispatch('logout')">store-logout-method-call</v-btn>
    <!-- script logout method call -->
    <v-btn @click="logout">script-logout-method-call</v-btn>
    <!-- ...mapActions(['logout']) call -->
    <v-btn @click="logout">mapActions-logout-method-call</v-btn>
  </div>
  ...
</template>
```

### `Vue Component script`

```javascript
import { mapState, mapGetters, mapActions, mapMutations } from "vuex";

export default {
  props: {
    ...
  },
  data: () => ({
    ...
  }),
  computed: {
    // ...mapState(  [" /* vuex store > state   group > variable    */ "]),
    // ...mapGetters([" /* vuex store > getters group > method call */ "])
    ...mapState(["count"]),
    ...mapGetters([])
  },
  methods: {
    // ...mapActions(  [" /* vuex store > actions   group > method call */ "]),
    // ...mapMutations([" /* vuex store > mutations group > method call */ "])
    ...mapActions(['logout']),
    ...mapMutations([])
    logout() {
      this.$store.dispatch("logout");
    }
  }
};
```

### `store.js`

```javascript
import Vue from "vue";
import Vuex from "vuex";
import router from "./router";
import axios from "axios";

Vue.use(Vuex);

export default new Vuex.Store({
  state: {
    ...
    count: 1
    ...
  },
  mutations: {
    ...
  },
  getters: {

  },
  actions: {
    ...
  }
});

```
