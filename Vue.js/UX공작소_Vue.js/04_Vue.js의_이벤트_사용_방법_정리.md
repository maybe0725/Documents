# Vue.js의 이벤트 사용 방법 정리

<br/>

원본글 출처

- [Vue.js의 이벤트 사용 방법 정리](https://ux.stories.pe.kr/119)

<br/>

Vue.js에서 이벤트를 다루는 방법에 대해서 간단히 정리하려고 합니다.
HTML에는 &lt;a&gt;나 &lt;input&gt; 처럼 당연히 알고 있는 기본 이벤트가 있습니다.
그리고 이미 javascript를 통해서 기본 이벤트와 어우러지게 사용하는 방법들이 있구요.
여기에서 Vue.js는 javascript로 조금 어렵게 구현했던 기능들을 v-on 디렉티브를 통해서 좀더 쉽게 구현할 수 있는 방법을 제공하고 있습니다.

<br/>

## 이벤트 구현

<br/>

Vue.js에서 이벤트를 구현하는 방법인 v-on 디렉티브를 이용합니다.
v-on은 줄여서 @로 대체할 수 있습니다.

```html
<!-- 해당 태그를 클릭 했을 경우 goodgood() 메소드 실행. -->
<button id="goodboy" v-on:click="goodgood">좋아요</button>
<button id="goodboy" @click="goodgood">좋아요</button>
```

좋아요 버튼을 클릭하면 javascript로 작성된 goodgood() 메서드를 호출합니다.

<br/>

### 기본 수식어

<table>
  <thead>
    <tr>
      <th>수식어 명</th>
      <th>설명</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>.prevent</td>
      <td>기본 이벤트의 자동 실행을 중단 시킴</td>
      <td>preventDefault() 과 동일한 기능</td>
    </tr>
    <tr>
      <td>.stop</td>
      <td>이벤트가 전파되는 것을 중단 시킴 (Bubbling 중단)</td>
      <td>stopPropagation() 과 동일한 기능</td>
    </tr>
    <tr>
      <td>.capture</td>
      <td>포착 단계에서만 이벤트를 발생시킴</td>
      <td>내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리함</td>
    </tr>
    <tr>
      <td>.self</td>
      <td>발생 단계에서만 이벤트를 발생시킴</td>
      <td>event.target이 엘리먼트 자체인 경우에만 트리거를 처리, 자식 엘리먼트에서는 실행안됨</td>
    </tr>
    <tr>
      <td>.once</td>
      <td>이벤트를 한번만 실행 시킴</td>
      <td></td>
    </tr>
    <tr>
      <td>.passive</td>
      <td>기본 이벤트를 취소할 수 없게 함</td>
      <td>있을지 모를 .preventDefault()를 실행 안되게 함.</td>
    </tr>
  </tbody>
</table>

```html
<!-- 스크롤의 기본 이벤트를 취소할 수 없습니다. -->
<div v-on:scroll.passive="onScroll">...</div>
```

<br/>

### 키 수식어

키(Key)에 대한 수식어는 원래 숫자를 이용하는 것이였으나 사람이 숫자를 기억하기 힘들기 때문에 키값에 해당하는 수식어를 별도의 이름으로 지정하여 활용합니다.

```html
<!-- keyCode가 13일 때만 `vm.submit()`을 호출합니다 -->
<input @:keyup.13="submit" />
<!-- 
  keyCode가 13 대신 .enter로 지정하여 활용합니다. 
  위의 코드와 동일하게 작동합니다. 
-->
<input @keyup.enter="submit" />
```

```html
<!-- 1개 키 사용법 -->
<input @click.enter="..." />
<input @keyup.tab="..." />
<input @keyup.space="..." />
```

```html
<!-- 여러 키 사용법  -->
<input @click.ctrl.enter="..." />
<input @keyup.alt.23="..." />
<input @keyup.shift.13="..." />
```

<table>
  <thead>
    <tr>
      <th>키 수식어 명</th>
      <th>고유 키 값</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>.enter</td>
      <td>13</td>
      <td></td>
    </tr>
    <tr>
      <td>.tab</td>
      <td>9</td>
      <td></td>
    </tr>
    <tr>
      <td>.delete</td>
      <td>8</td>
      <td>“Delete” 와 “Backspace” 키 모두 해당</td>
    </tr>
    <tr>
      <td>.esc</td>
      <td>27</td>
      <td></td>
    </tr>
    <tr>
      <td>.space</td>
      <td>32</td>
      <td></td>
    </tr>
    <tr>
      <td>.up</td>
      <td>33</td>
      <td></td>
    </tr>
    <tr>
      <td>.down</td>
      <td>34</td>
      <td></td>
    </tr>
    <tr>
      <td>.left</td>
      <td>37</td>
      <td></td>
    </tr>
    <tr>
      <td>.right</td>
      <td>39</td>
      <td></td>
    </tr>
    <tr>
      <td>.ctrl</td>
      <td>17</td>
      <td></td>
    </tr>
    <tr>
      <td>.alt</td>
      <td>18</td>
      <td></td>
    </tr>
    <tr>
      <td>.shift</td>
      <td>16</td>
      <td></td>
    </tr>
    <tr>
      <td>.meta</td>
      <td>16</td>
      <td>매킨토시에서 command 키, Windows에서는 windows 키</td>
    </tr>
  </tbody>
</table>

그 외의 키들은 개별로 커스텀하게 제작하여 사용할 수 있습니다.

<br/>

### 마우스 버튼 수식어

키(Key) 수식어 처럼 마우스 버튼에 대한 수식어도 아래와 같습니다.

<table>
  <thead>
    <tr>
      <th>키 수식어 명</th>
      <th>설명</th>
      <th>비고</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>.left</td>
      <td>마우스 왼쪽 버튼 클릭</td>
      <td></td>
    </tr>
    <tr>
      <td>.right</td>
      <td>마우스 오른쪽 버튼 클릭</td>
      <td></td>
    </tr>
    <tr>
      <td>.middle</td>
      <td>마우스 가운데 휠 버튼 클릭</td>
      <td></td>
    </tr>
  </tbody>
</table>

```html
<!-- 마우스 오버 후 오른쪽 버튼 클릭을 했을 경우 `goodgood()`을 호출합니다 -->
<div id="goodboy" v-on:mouseover.left="goodgood"></div>
<!-- 마우스 오버 후 왼쪽 버튼 클릭 했을 경우 `goodgood()`을 호출합니다 -->
<div id="goodboy" @mouseover.right="goodgood"></div>
```
