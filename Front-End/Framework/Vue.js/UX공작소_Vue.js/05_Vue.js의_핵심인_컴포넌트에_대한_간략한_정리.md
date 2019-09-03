# Vue.js의 핵심인 컴포넌트에 대한 간략한 정리

<br/>

원본글 출처

- [Vue.js의 핵심인 컴포넌트에 대한 간략한 정리](https://ux.stories.pe.kr/131)

<br/>

Vue.js의 핵심이자 꽃이라 할 수 있는 컴포넌트(Component)에 대해 간략히 정리해 보려고 합니다.

![images](https://t1.daumcdn.net/cfile/tistory/9993B64A5C63CB8F21)

`컴포넌트 구성 (출처:vuejs.org)`

Vue.js에서 기본적인 프로그램 구현 방법은 화면 또는 기능을 적절하게 분리하여 컴포넌트화 시켜서 개발을 하고 이것을 연결하여 프로그램이 작동하도록 구현하는 방법을 사용합니다.

![images](https://t1.daumcdn.net/cfile/tistory/993C06365C63CB8F24)

`컴포넌트간 데이터 전달방식 (출처:vuejs.org)`

또한 각각의 컴포넌트간 데이터의 이동에 대해서는 `단방향 이동만 가능`하다고 합니다.
부모 컴포넌트에 있는 데이터를 자식 컴포넌트가 가져올 때는 그냥 쉽게 부모 컴포넌트에 props로 변수에 값을 넣어주면 바로 자식 컴포넌트가 읽어 올 수 있습니다.

가진 것을 모두 퍼 주는 부모님의 내리사랑 같은거에요.

하지만 자식 컴포넌트에 있는 데이터를 부모 컴포넌트에 넘기기 위해서는 기본적으로는 넘길 수 없고 Event를 사용해서 보내 줘야지만 넘길 수 있습니다.
엄마 통장으로 돈 부쳤어 살림에 보테~

컴포넌트를 사용할 경우 장점은 `재사용성이 매우 뛰어 납니다.`
한번 작성한 컴포넌트는 다른 곳에서 여러번 사용할 수 있습니다.
단위 테스트가 용이 합니다.
테스트를 컴포넌트 단위로 하면 되기 때문에 명확하고 쉽게 수정도 가능하게 됩니다.

<br/>

## 컴포넌트 등록 방법

컴포넌트는 전역으로 등록 할 수도 있고 지역으로 등록할 수도 있습니다.
전역으로 등록할 경우 모든 자식 컴포넌트에서 해당 컴포넌트를 사용할 수 있습니다.
하지만 전역컴포넌트를 남용할 경우는 결과물이 상당히 커지는 사례가 생길 수 있습니다.
지역으로 등록할 경우는 일단 만들어 놓은 지역 컴포넌트를 불러와서 사용 할 수 있습니다.
그리고 전역변수와 달리 자식 컴포넌트에서는 자동으로 사용할 수 가 없습니다.
자식 컴포넌트에서 사용하려면 다시 자식 컴포넌트에서 불러와서 사용해야 합니다.
필요할 경우 컴포넌트를 불러와야 하는 불편함이 있지만 결과물 코드용량을 줄일 수 있는 잇점이 있습니다.

<br/>

### 전역 컴포넌트 등록

```js
Vue.component("컴포넌트 이름", "옵션");
```

- 컴포넌트 이름 : 이 컴포넌트를 사용하기 위한 이름입니다.
- 옵션 : 컴포넌트를 렌더링할 때 필요한 여러 옵션들을 작성합니다.

```js
Vue.component("component-a", { template: "<div> hello world AAA !!!</div>" });

Vue.component("component-b", { template: "<div> hello world BBB !!!</div>" });

Vue.component("component-c", { template: "<div> hello world CCC !!!</div>" });

new Vue({ el: "#app" });
```

실제로 컴포넌트는 위와 같이 등록합니다.

```html
<div id="app">
  <component-a></component-a>
  <component-b></component-b>
</div>
```

등록된 컴포넌트는 위와 같이 컴포넌트 이름을 사용합니다.

```html
<div id="app">
  <div>hello world AAA !!!</div>
  <div>hello world BBB !!!</div>
</div>
```

그러면 렌더링 후에는 위와 같이 구현됩니다.

<br/>

### 지역 컴포넌트 등록

```js
var ComponentD = { template: "<div> hello world DDD !!!</div>" };
var ComponentE = { template: "<div> hello world EEE !!!</div>" };
var ComponentF = { template: "<div> hello world FFF !!!</div>" };
```

지역 컴포넌트는 위와같이 등록합니다.

```js
new Vue({
  el: "#app",
  components: {
    "component-d": ComponentD,
    "component-e": ComponentE
  }
});
```

사용할 컴포넌트 들만 등록하여 사용합니다.

```html
<div id="app">
  <component-d></component-d>
  <component-e></component-e>
</div>
```

등록된 컴포넌트를 사용하는 것은 전역이나 지역이나 동일 합니다.

```html
<div id="app">
  <div>hello world DDD !!!</div>
  <div>hello world EEE !!!</div>
</div>
```

그러면 렌더링 후에는 위와 같이 구현됩니다.

<br/>

## 컴포넌트 옵션 요소

<br/>

<table>
  <thead>
    <tr>
      <th>옵션</th>
      <th>설명</th>
      <th>예제</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>template</td>
      <td>해당 컴포넌트의 DOM형태를 구성</td>
      <td>template : '&lt;div&gt;hello&lt;/div&gt;'</td>
      <td></td>
    </tr>
    <tr>
      <td>data</td>
      <td>컴포넌트가 바라보는 data 객체를 Function 형태로 지정</td>
      <td>data : function() {return {msg: "hello"}}</td>
      <td>객체를 직접 사용할 수 없고 Functio으로 객체를 리턴시켜 사용해야함</td>
    </tr>
    <tr>
      <td>props</td>
      <td>자식 컴포넌트에게 값을 전달하기 위해 props 배열 또는 객체를 지정하여 사용</td>
      <td>props: ['msg']</td>
      <td>props: {msg: {type: String, default: 'Hello'}}</td>
    </tr>
    <tr>
      <td>methods</td>
      <td>자식 컴포넌트에서 부모 컴포넌트로 이벤트 함수로 값을 넘길때 주로 사용함</td>
      <td>methods: {eventCl: function(e) {this.$emit('parentPn', e.target.innerText);}}</td>
      <td></td>
    </tr>
  </tbody>
</table>
