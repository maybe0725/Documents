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

<br/>

## Sample Source

<br/>

### `store.js`

```js
/* store.js */
import Vue from 'vue'
import Vuex from 'vuex'
import axios from 'axios'

Vue.use(Vuex);

export default new Vuex.Store({
    state: {
        count: 0,
        weight: 2,
        random: 0,
    },
    mutations: {
        increment(state) {
            state.count++;
        },
        decrement(state) {
            state.count--;
        },
        successGenerateRandomNumber(state, payload){
            state.random = payload.num;
        },
        failGenerateRandomNumber(/*state, payload*/){
            console.log('ERROR!');
        }
    },
    getters:{
        count(state, getters){
            return Math.pow(state.count, getters.weight);
        },
        weight(state, /*getters*/){
            return state.weight;
        },
        random(state, /*getters*/){
            return state.random;
        }
    },
    actions:{
        generateRandomNumber({commit, /*state*/}, /*payload*/) {
            axios.get(`http://localhost:4321/`)
                .then((res) => {
                    commit('successGenerateRandomNumber', res.data);
                })
                .catch((res) => {
                    commit('failGenerateRandomNumber', res);
                });
        }
    }
})
```

### `HelloWorld.vue`

```html
<!--HelloWorld.vue-->
<template>
    <div class="hello">
        <b>count : {{$store.state.count}}</b><br>
        <b>count^2 : {{$store.getters.count}}</b><br>
        <b>random : {{$store.getters.random}}</b><br>
        <input type="button" @click="increment()" value="increment"/>
        <input type="button" @click="decrement()" value="decrement"/>
        <input type="button" @click="randomNumber()" value="random"/>
    </div>
</template>

<script>
    export default {
        name: 'HelloWorld',
        data() {
            return {}
        },
        created: function () {
        },
        methods: {
            increment: function () {
                this.$store.commit('increment')
            },
            decrement: function () {
                this.$store.commit('decrement')
            },
            randomNumber: function () {
                this.$store.dispatch('generateRandomNumber', /*100*/);
            }
        }
    }
</script>

<style scoped>
</style>
```
