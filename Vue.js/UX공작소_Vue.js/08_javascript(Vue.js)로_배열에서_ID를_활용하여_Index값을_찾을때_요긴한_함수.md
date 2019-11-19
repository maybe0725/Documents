# javascript(Vue.js)로 배열에서 ID를 활용하여 Index값을 찾을때 요긴한 함수

<br/>

원본글 출처

- [javascript(Vue.js)로 배열에서 ID를 활용하여 Index값을 찾을때 요긴한 함수](https://ux.stories.pe.kr/135)

<br/>

Vue.js에서 배열에 있는 특정 id의 Index값을 찾아서 그 배열을 처리하는 요긴하면서 쉬운 함수를 공유합니다.

아래의 예제는 todolist배열을 화면에 나열해 놓고 그 중에 항목을 하나 클릭할 경우 doneToggle(id) 또는 deleteTodo(id)를 불러와서 반대 값으로 토글 시키거나 항목을 삭제하는 함수입니다.

```html
<ul id="todolist">
  <li
    v-for="a in todolist"
    v-bind:class="checked(a.done)"
    v-on:click="doneToggle(a.id)"
  >
    <span>{{ a.todo }}</span> <span v-if="a.done"> (완료) </span>
    <span class="close" v-on:click.stop="deleteTodo(a.id)">&#x00D7;</span>
  </li>
</ul>
```

```js
doneToggle : function(id) {
  var index = this.todolist.findIndex(function(item){
    return item.id === id;
  })

  this.todolist[index].done = !this.todolist[index].done
}
```

핵심은 findIndex() 함수입니다.
함수에서 받은 id값을 findIndex()를 활용하여 배열 안에 있는 item.id와 비교하여 동일한 경우 해당 값을 var index 변수에 집어 넣는 역활을 합니다.

이 구해 진 index값을 활용하여 todolist의 배열 위치를 찾아 done 변수에 있는 값을 반대 값으로 토글 시켜 줍니다.

아래의 예제도 index값을 구하는 방법은 동일합니다.

```js
doneToggle : function(id) {
  var index = this.todolist.findIndex(function(item){
    return item.id === id;
  })

  this.todolist.splice(index,1);
}
```

위와 동일한 방법으로 구해진 index값을 활용하여 dotolist배열에서 splice()함수를 활용하여 삭제를 시키는 코드입니다.

아래와 같이 특정위치의 배열 값을 구하는 함수를 기억해 놓으면 요긴하게 사용 할 수 있습니다.

```js
var index = this.todolist.findIndex(function(item) {
  return item.id === id;
});
```
