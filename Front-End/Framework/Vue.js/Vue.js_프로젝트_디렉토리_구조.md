# Vue.js_프로젝트_디렉토리_구조

참고

- [Vue.js 개발에 적합한 디렉토리 구조](https://github.com/vuejs-kr/vuejs-kr.github.io/issues/28)
- http://vuejs-templates.github.io/webpack/structure.html
- [Vue.js 2 + Vuex + Router + yarn! Basic Configuration version 2](https://medium.com/tldr-tech/vue-js-2-vuex-router-yarn-basic-configuration-version-2-7b9c489d43b3)
- [GitHub - Vue.js Component Style Guide](https://github.com/pablohpsilva/vuejs-component-style-guide)
- [The medium article - Vue.js Component Style Guide](https://medium.com/tldr-tech/vue-js-component-style-guide-711988d5e94e#.tjuxa5qmu)

```
src
├── App.vue
├── assets
|   ├── css
|   |   └── main.css
|   ├── font
|   └── img
├── commons
|   ├── directives
|   ├── functions
|   ├── resources
|   └── validations
├── config
|   ├── directives.js
|   ├── router.js
|   └── validations.js
├── shared-components
|   ├── RangeCustom.vue
|   ├── Sidebar.vue
|   └── Toolbar.vue
├── spa
|   ├── Login
|   |   ├── components
|   |   └── Login.vue
|   ├── Products
|   |   ├── components
|   |   └── Products.vue
|   ├── components
|   ├── Home.vue
|   └── NotFound.vue
├── vuex
|   ├── modules
|   └── store.js
└── main.js
```

<br/>

## views와 components의 차이

router에서 보여주는 component 파일은 views 폴더에 넣는다.

그 외에는 components 폴더다.

- 참고 : https://stackoverflow.com/questions/50865828/what-is-the-difference-between-the-views-and-components-folders-in-a-vue-project
