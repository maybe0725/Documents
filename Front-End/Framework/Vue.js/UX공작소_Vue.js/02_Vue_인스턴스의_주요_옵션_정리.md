# Vue 인스턴스의 주요 옵션 정리

<br/>

원본글 출처

- [Vue 인스턴스의 주요 옵션 정리](https://ux.stories.pe.kr/113)

<br/>

Vue를 실행시기키 위해서는 먼저 Vue.js를 import를 한 후 **Vue 인스턴스를 생성**해야 합니다.
Vue인스턴스를 생성할때 지정된 몇가지 옵션 객체를 전달해야 하는데 이 옵션객체들이 어떤것이 있고 어떻게 사용되는지 간단히 정리하려고 합니다.

<br/>

## Vue 인스턴스란?

<br/>

Vue를 실행하기 위해 첫번째로 정의 하고 생성해야 하는 객체입니다.

<br/>

### Vue 인스턴스 예제

```js
var vm = new Vue({
  el: "#test",
  data: { name: "홍길동" }
});
```

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
      <td>el</td>
      <td>Vue가 실행될 HTML의 DOM 요소를 지정</td>
      <td>el : '#test'</td>
      <td>CSS의 선택자를 선택하듯이 선택 (#--> id 지정, . --> 클레스 지정)</td>
    </tr>
    <tr>
      <td>data</td>
      <td>Vue가 바라보는 data 객체를 지정</td>
      <td>data : { name : '홍길동'}</td>
      <td>직접 객체를 작성해도 되고 미리 선언된 객체변수를 작성해도 됨</td>
    </tr>
    <tr>
      <td>computed</td>
      <td>함수로 정의하고 data객체 등을 사용하여 계산된 값을 리턴해 줌. methods와 차이점은 캐싱을 시켜놓고 동일한 요청이 또 올 경우는 함수를 실행하지 않고 캐싱된 값만 리턴해 줌</td>
      <td>computed : { sum1 : function() {retuen 3 + 4}}</td>
      <td>화살표함수는 사용 불가</td>
    </tr>
    <tr>
      <td>methods</td>
      <td>함수로 정의하고 data객체 등을 사용하여 계산된 값을 리턴해 줌. computed와 차이점은 캐싱이 되지 않고 호출될때마다 계속 함수를 실행함</td>
      <td>methods : { sum2 : function() {retuen 3 + 4}}</td>
      <td>화살표함수는 사용 불가</td>
    </tr>
    <tr>
      <td>watch</td>
      <td>지정된 변수를 계속 지켜보고 있다가 값이 변경되었을때 정의된 함수를 실행시킴</td>
      <td>watch : { x : function(v) {retuen v++ }}</td>
      <td>x는 관찰하고자 하는 지정된 변수, 긴 시간이 필요한 비동기 처리가 필요할 경우 주로 사용됨(axios, fetch 등등..)</td>
    </tr>
  </tbody>
  </table>
