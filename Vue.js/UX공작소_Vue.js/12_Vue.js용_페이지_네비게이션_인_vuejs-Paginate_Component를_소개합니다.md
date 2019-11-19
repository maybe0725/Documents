# Vue.js용 페이지 네비게이션 인 vuejs-Paginate Component를 소개합니다

<br/>

원본글 출처

- [Vue.js용 페이지 네비게이션 인 vuejs-Paginate Component를 소개합니다](https://ux.stories.pe.kr/139)

<br/>

Vue.js를 사용하다보면 수많은 사람들이 만들어서 무료로 공유해 놓은 Components들이 많이 있습니다. 라이선스를 꼭 확인해야 하겠지만 대부분 사용에 안전한 MIT라이선스를 주로 사용합니다.

그중에 보통 게시판 리스트 하단에 페이지를 넘기는데 사용하는 Paginate 컴포넌트를 소개해 볼까 합니다.

![images](https://t1.daumcdn.net/cfile/tistory/99CA314B5CC031771F)

이 컴포넌트는 디자인은 Bootstrap3을 사용하고 있기때문에 Bootstrap3도 같이 설치를 해야 합니다.

<br/>

## 설치하기

<br/>

npm이나 yarn을 사용한다면 설치는 쉽습니다.

<br/>

### NPM으로 설치

```sh
$ npm install vuejs-paginate bootstrap@3.3.x --save
```

<br/>

### Yarn으로 설치

```sh
$ yarn add vuejs-paginate bootstrap@3.3.x
```

<br/>

### CDN으로 설치

CDN은 설치라기 보다는 HTML의 &lt;HEAD&gt;&lt;/HEAD&gt;영역에 아래와 같이 링크로 걸어 주면 됩니다.

```html
<!-- use the latest release -->
<script src="https://unpkg.com/vuejs-paginate@latest"></script>
```

<br/>

## 적용하기

<br/>

사용하는 방법은 아래와 같습니다.

<br/>

### javascript ES5 버전에서 적용하기

Vue 파일의 `script`영역에 아래와 같이 `vuejs-paginate`를 불러와서 컴포넌트에 등록을 합니다.

ES5버전에서는 require()로 불러와야 합니다.

```js
var Paginate = require("vuejs-paginate");
Vue.component("paginate", Paginate);
```

<br/>

### javascript ES6 버전에서 적용하기

require()도 사용가능하지만,

ES6버전에서는 import ~~ from ~~으로 불러오는 것이 표준입니다.

```js
import Paginate from "vuejs-paginate";
Vue.component("paginate", Paginate);
```

<br/>

### CDN으로 적용하기

위에서 CDN으로 HTML에 불러왔다면 javascript에서 컴포넌트를 꼭 등록해야 합니다.

```js
Vue.component("paginate", VuejsPaginate);
```

<br/>

## 사용 하기

<br/>

등록하고 적용까지 했으니 이제 실제로 사용하면 됩니다.

이 컴포넌트의 사용은 Vue파일 Template영역의 원하는 자리에 사용을 하면 됩니다.

<br/>

### 기본형태

아래는 기본형태 입니다.

```html
<paginate
  :page-count="20"
  :click-handler="functionName"
  :prev-text="'Prev'"
  :next-text="'Next'"
  :container-class="'className'"
>
</paginate>
```

<br/>

## 샘플보기

<br/>

전체적으로 어떻게 사용하는지 샘플은 아래와 같습니다.

하나의 Vue파일이며 template영역, script영역, style영역으로 나누어져있습니다.

```html
<template>
  <paginate
    v-model="page"
    :page-count="20"
    :page-range="3"
    :margin-pages="2"
    :click-handler="clickCallback"
    :prev-text="'Prev'"
    :next-text="'Next'"
    :container-class="'pagination'"
    :page-class="'page-item'"
  >
  </paginate>
</template>

<script>
  import Paginate from 'vuejs-paginate'
  Vue.component('paginate', Paginate)

  export default {
    data() {
      return {
        page: 10
      }
    },
    methods: {
      clickCallback (pageNum) => {
        console.log(pageNum)
      }
    }
  }
</script>

<style lang="css">
  .pagination { }
  .page-item { }
</style>
```

더 자세한 정보는 아래의 github사이트를 참조하시면 됩니다.

> https://github.com/lokyoung/vuejs-paginate
